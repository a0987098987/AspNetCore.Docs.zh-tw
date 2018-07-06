---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: 在 Visual Studio 2013 的 ASP.NET Scaffold |Microsoft Docs
author: tfitzmac
description: ASP.NET Scaffold 是隨附於 Visual Studio 2013 中的新功能。
ms.author: aspnetcontent
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 2fcfc31069e8fee79eb217ef6bad746f85a085e0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839564"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="867b5-103">在 Visual Studio 2013 的 ASP.NET Scaffold</span><span class="sxs-lookup"><span data-stu-id="867b5-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="867b5-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="867b5-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="867b5-105">ASP.NET Scaffold 是隨附於 Visual Studio 2013 中的新功能。</span><span class="sxs-lookup"><span data-stu-id="867b5-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="867b5-106">總覽</span><span class="sxs-lookup"><span data-stu-id="867b5-106">Overview</span></span>

<span data-ttu-id="867b5-107">ASP.NET Scaffold 是 ASP.NET Web 應用程式的程式碼產生架構。</span><span class="sxs-lookup"><span data-stu-id="867b5-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="867b5-108">Visual Studio 2013 包含預先安裝的程式碼產生器，適用於 MVC 和 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="867b5-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="867b5-109">當您想要快速新增資料模型進行互動的程式碼時，您可以新增至您專案的 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="867b5-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="867b5-110">使用 scaffolding 可減少開發您的專案中的標準資料作業的時間量。</span><span class="sxs-lookup"><span data-stu-id="867b5-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="867b5-111">根據預設，Visual Studio 2013 不支援的 Web Form 專案，產生的程式碼，但您可以藉由將 MVC 相依性新增至專案，或安裝延伸模組 Web form 使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="867b5-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="867b5-112">這兩種方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="867b5-112">Both approaches are shown below.</span></span>

<span data-ttu-id="867b5-113">Visual Studio 2013 Update 2 (目前 RC) 提供功能來擴充 ASP.NET Scaffolding，以符合您案例的需求。</span><span class="sxs-lookup"><span data-stu-id="867b5-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="867b5-114">透過這項功能，您可以建立自訂的 scaffolding 範本，並將它新增至加入新的 Scaffold 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="867b5-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="867b5-115">在自訂的範本中，您可以指定加入 scaffold 項目時，會產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="867b5-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="867b5-116">如需詳細資訊，請參閱 <<c0> [ 適用於 Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。</span><span class="sxs-lookup"><span data-stu-id="867b5-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="867b5-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="867b5-117">Prerequisites</span></span>

<span data-ttu-id="867b5-118">若要使用的 ASP.NET Scaffold，您必須具備：</span><span class="sxs-lookup"><span data-stu-id="867b5-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="867b5-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="867b5-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="867b5-120">Web 開發人員工具 （預設 Visual Studio 2013 安裝的一部分）</span><span class="sxs-lookup"><span data-stu-id="867b5-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="867b5-121">ASP.NET Web 架構與工具的 2013 （預設 Visual Studio 2013 安裝的一部分）</span><span class="sxs-lookup"><span data-stu-id="867b5-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="867b5-122">將 scaffold 項目新增至 MVC 或 Web API</span><span class="sxs-lookup"><span data-stu-id="867b5-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="867b5-123">若要加入 scaffold，以滑鼠右鍵按一下專案或資料夾，以在專案中，然後選取**新增**–**新增 Scaffold 項目**，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="867b5-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![加入 scaffold 項目](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="867b5-125">從**新增 Scaffold** ] 視窗中，選取 [新增 scaffold 的型別。</span><span class="sxs-lookup"><span data-stu-id="867b5-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![選取的 scaffold 的類型](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="867b5-127">**新增控制器**視窗可讓您選取選項來產生控制器，包括您是否想要使用 Entity Framework 6 的新非同步功能。</span><span class="sxs-lookup"><span data-stu-id="867b5-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![新增控制器](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="867b5-129">頁面與相關的類別會針對您的案例。</span><span class="sxs-lookup"><span data-stu-id="867b5-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="867b5-130">例如下, 圖顯示的 MVC 控制器和透過名為電影模型類別的 scaffolding 所建立的檢視。</span><span class="sxs-lookup"><span data-stu-id="867b5-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![建立的檔案](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="867b5-132">加入 Web Form 中的 scaffold 項目</span><span class="sxs-lookup"><span data-stu-id="867b5-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="867b5-133">產生 Web Form 的程式碼的樣板加入，您必須安裝 Visual studio 擴充功能，或新增 MVC 相依性。</span><span class="sxs-lookup"><span data-stu-id="867b5-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="867b5-134">這兩種方法如下所示，但您只需要執行其中一種方法。</span><span class="sxs-lookup"><span data-stu-id="867b5-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="867b5-135">Web Forms Scaffolding 擴充功能</span><span class="sxs-lookup"><span data-stu-id="867b5-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="867b5-136">您可以安裝 Visual Studio 擴充功能可讓您使用 Web Form 專案中使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="867b5-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="867b5-137">在 Visual Studio 中，選取**工具**，然後**擴充功能和更新**。</span><span class="sxs-lookup"><span data-stu-id="867b5-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="867b5-138">在此對話方塊中搜尋 Visual Studio 元件庫**Web Forms Scaffolding**。</span><span class="sxs-lookup"><span data-stu-id="867b5-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![安裝 web forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="867b5-140">如需詳細資訊，請參閱 < [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478)。</span><span class="sxs-lookup"><span data-stu-id="867b5-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="867b5-141">MVC 相依性</span><span class="sxs-lookup"><span data-stu-id="867b5-141">MVC Dependencies</span></span>

<span data-ttu-id="867b5-142">若要新增 MVC 相依性，請選取**新增** - **新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="867b5-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="867b5-143">在 [新增 Scaffold] 視窗中，選取**MVC 相依性**，如下所示。</span><span class="sxs-lookup"><span data-stu-id="867b5-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![新增 MVC 相依性](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="867b5-145">有兩個選項，scaffolding MVC;最少且完整。</span><span class="sxs-lookup"><span data-stu-id="867b5-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="867b5-146">如果您選取最小，只有 NuGet 套件和 ASP.NET mvc 的參考會加入到專案中。</span><span class="sxs-lookup"><span data-stu-id="867b5-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="867b5-147">如果您選取 [完整] 選項中，加入基本的相依性，以及 MVC 專案所需的內容檔案。</span><span class="sxs-lookup"><span data-stu-id="867b5-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="867b5-148">若要輕鬆地使用 scaffolding，選取 完整相依性。</span><span class="sxs-lookup"><span data-stu-id="867b5-148">To easily use scaffolding, select Full dependencies.</span></span>

![選取 完整相依性](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="867b5-150">新增相依性之後, 您會看到**readme.txt**檔案。</span><span class="sxs-lookup"><span data-stu-id="867b5-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="867b5-151">請仔細依照此檔案中的指示，以確保您的專案可以正確運作。</span><span class="sxs-lookup"><span data-stu-id="867b5-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="867b5-152">當您完成 readme.txt 檔案中的步驟時，您可以加入新的 scaffold 項目，有關 MVC 和 Web API 上一節中所示。</span><span class="sxs-lookup"><span data-stu-id="867b5-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="867b5-153">自動產生的檢視和控制器，則會在專案中正確運作。</span><span class="sxs-lookup"><span data-stu-id="867b5-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="867b5-154">教學課程</span><span class="sxs-lookup"><span data-stu-id="867b5-154">Tutorials</span></span>

<span data-ttu-id="867b5-155">若要建立自訂的 scaffolder，請參閱[適用於 Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。</span><span class="sxs-lookup"><span data-stu-id="867b5-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="867b5-156">若要自訂產生的檔案，請參閱[如何自訂產生的檔案，從 [新增 Scaffold 項目] 對話方塊](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)。</span><span class="sxs-lookup"><span data-stu-id="867b5-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="867b5-157">如需範例使用 scaffolding **Database First 開發**，請參閱[EF Database First 與 ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)。</span><span class="sxs-lookup"><span data-stu-id="867b5-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="867b5-158">如需範例的使用中的 scaffolding **MVC**專案，請參閱[Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="867b5-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="867b5-159">如需範例的使用中的 scaffolding **Web API**專案，請參閱[建立 REST API 與 Web API 2 中的屬性路由](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)。</span><span class="sxs-lookup"><span data-stu-id="867b5-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
