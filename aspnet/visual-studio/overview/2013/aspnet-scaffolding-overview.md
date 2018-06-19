---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013 中的 ASP.NET Scaffolding |Microsoft 文件
author: tfitzmac
description: ASP.NET Scaffolding 是隨附於 Visual Studio 2013 的新功能。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506727"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="1dd7f-103">Visual Studio 2013 中的 ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="1dd7f-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="1dd7f-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1dd7f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1dd7f-105">ASP.NET Scaffolding 是隨附於 Visual Studio 2013 的新功能。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="1dd7f-106">概觀</span><span class="sxs-lookup"><span data-stu-id="1dd7f-106">Overview</span></span>

<span data-ttu-id="1dd7f-107">ASP.NET Scaffolding 是 ASP.NET Web 應用程式的程式碼產生架構。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="1dd7f-108">Visual Studio 2013 包含預先安裝的程式碼產生器適用於 MVC 和 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="1dd7f-109">當您想要快速加入資料模型互動的程式碼時，您可以加入至您的專案的 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="1dd7f-110">使用 scaffolding 可減少開發您的專案中的標準資料作業的時間量。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="1dd7f-111">根據預設，Visual Studio 2013 不支援的 Web Form 專案，產生程式碼，但您可以使用 scaffolding Web 表單將 MVC 相依性加入專案，或安裝擴充功能。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="1dd7f-112">這兩種方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-112">Both approaches are shown below.</span></span>

<span data-ttu-id="1dd7f-113">Visual Studio 2013 Update 2 (目前 RC) 提供功能延長 ASP.NET Scaffolding，以符合您案例的需求。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="1dd7f-114">透過這項功能，您可以建立自訂的 scaffolding 範本，並將它加入至加入新的 Scaffold 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="1dd7f-115">在自訂的範本，您可以指定可以加入 scaffold 項目時，會產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="1dd7f-116">如需詳細資訊，請參閱[for Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dd7f-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="1dd7f-117">Prerequisites</span></span>

<span data-ttu-id="1dd7f-118">若要使用 ASP.NET Scaffolding，您必須：</span><span class="sxs-lookup"><span data-stu-id="1dd7f-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="1dd7f-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1dd7f-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="1dd7f-120">Web 開發人員工具 （預設 Visual Studio 2013 安裝的一部分）</span><span class="sxs-lookup"><span data-stu-id="1dd7f-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="1dd7f-121">ASP.NET 網頁架構和工具 2013 （預設 Visual Studio 2013 安裝的一部分）</span><span class="sxs-lookup"><span data-stu-id="1dd7f-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="1dd7f-122">將 scaffold 項目加入至 MVC 或 Web API</span><span class="sxs-lookup"><span data-stu-id="1dd7f-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="1dd7f-123">若要加入 scaffold，以滑鼠右鍵按一下專案或專案內的資料夾，然後選取**新增**–**新的 Scaffold 項目**，如下列影像所示。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![加入 scaffold 項目](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="1dd7f-125">從**新增 Scaffold**視窗中，選取要加入 scaffold 的類型。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![選取 scaffold 類型](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="1dd7f-127">**加入控制器**視窗讓您選取選項來產生控制器，包括您是否想要使用新的非同步功能，從 Entity Framework 6 的機會。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![加入控制器](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="1dd7f-129">相關類別和頁面會建立您的案例。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="1dd7f-130">例如下, 圖顯示的 MVC 控制器和透過模型類別，名為影片的 scaffolding 所建立的檢視。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![建立的檔案](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="1dd7f-132">Web Form 中加入 scaffold 項目</span><span class="sxs-lookup"><span data-stu-id="1dd7f-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="1dd7f-133">Web Form 程式碼會產生的樣板加入，您必須安裝 Visual studio 擴充功能，或將 MVC 相依性。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="1dd7f-134">這兩種方法如下所示，但您只需要執行其中一種方法。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="1dd7f-135">Web Form 的 Scaffolding 擴充功能</span><span class="sxs-lookup"><span data-stu-id="1dd7f-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="1dd7f-136">您可以安裝 Visual Studio 擴充功能可讓您使用 Web Form 專案中使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="1dd7f-137">在 Visual Studio 中，選取**工具**然後**擴充功能和更新**。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="1dd7f-138">在此對話方塊中搜尋 Visual Studio 組件庫的**Web Form Scaffolding**。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![安裝 web form 的 scaffolding](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="1dd7f-140">如需詳細資訊，請參閱[Web Form Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="1dd7f-141">MVC 相依性</span><span class="sxs-lookup"><span data-stu-id="1dd7f-141">MVC Dependencies</span></span>

<span data-ttu-id="1dd7f-142">若要將 MVC 相依性，請選取**新增** - **新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="1dd7f-143">在 [新增 Scaffold] 視窗中，選取**MVC 相依性**，如下所示。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![將 MVC 相依性](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="1dd7f-145">有兩個選項的 scaffolding MVC;最少且完整。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="1dd7f-146">如果您選取最少，只有 NuGet 封裝和 ASP.NET MVC 的參考會加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="1dd7f-147">如果您選取 [完整] 選項，加入最少的相依性，以及所需的內容檔案的 MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="1dd7f-148">若要輕鬆地使用 scaffolding，選取 完整的相依性。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-148">To easily use scaffolding, select Full dependencies.</span></span>

![選取完整的相依性](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="1dd7f-150">在之後新增相依性，您會看到**readme.txt**檔案。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="1dd7f-151">小心遵循此檔案中的指示，以確保您的專案可以正確運作。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="1dd7f-152">當您完成的 readme.txt 檔案中的步驟時，您可以加入新的 scaffold 項目，如前一節有關 MVC 和 Web API 中所示。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="1dd7f-153">自動產生的檢視和控制器，則將您的專案中正確運作。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="1dd7f-154">教學課程</span><span class="sxs-lookup"><span data-stu-id="1dd7f-154">Tutorials</span></span>

<span data-ttu-id="1dd7f-155">若要建立自訂的 scaffolder，請參閱[for Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="1dd7f-156">若要自訂產生的檔案，請參閱[如何自訂產生的檔案從新的 Scaffold 項目對話方塊](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="1dd7f-157">如需使用 scaffolding **Database First 開發**，請參閱[EF Database First 搭配 ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="1dd7f-158">如需範例的使用中的 scaffolding **MVC**專案，請參閱[開始使用 ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="1dd7f-159">如需範例的使用中的 scaffolding **Web API**專案，請參閱[屬由路由的 Web API 2 中建立 REST API](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)。</span><span class="sxs-lookup"><span data-stu-id="1dd7f-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
