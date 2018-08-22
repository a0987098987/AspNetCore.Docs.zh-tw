---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 基本概念 |Microsoft Docs
author: rick-anderson
description: 這個實際操作實驗室根據 MVC （模型檢視控制器） 音樂市集，介紹，並逐步說明如何使用 ASP.NET MV 的教學課程應用程式...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: d8e837a5d56871d271590859c2e82336111cc87a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835032"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="004e1-103">ASP.NET MVC 4 基本概念</span><span class="sxs-lookup"><span data-stu-id="004e1-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="004e1-104">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="004e1-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="004e1-105">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="004e1-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="004e1-106">這個實際操作實驗室是以 MVC （模型檢視控制器） 音樂市集，教學課程的應用程式的介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 為基礎。</span><span class="sxs-lookup"><span data-stu-id="004e1-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="004e1-107">在實驗室中，您將學習簡單起見，尚未一起使用這些技術的電源。</span><span class="sxs-lookup"><span data-stu-id="004e1-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="004e1-108">您將使用簡單的應用程式啟動，而且將會建置它除非您有功能完整 ASP.NET MVC 4 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="004e1-109">這個實驗室會搭配 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="004e1-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="004e1-110">如果您想要探索 ASP.NET MVC 3 版的教學課程的應用程式，您可以找到它[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="004e1-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="004e1-111">這個實際操作實驗室會假設，開發人員會有 Web 開發技術，例如 HTML 和 JavaScript 的經驗。</span><span class="sxs-lookup"><span data-stu-id="004e1-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="004e1-112">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="004e1-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="004e1-113">這個實驗室中的特定專案將會位於[ASP.NET MVC 4 基本概念](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)。</span><span class="sxs-lookup"><span data-stu-id="004e1-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="004e1-114">音樂市集應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-114">The Music Store application</span></span>

<span data-ttu-id="004e1-115">將會建置在本實驗室中的音樂市集 web 應用程式包含三個主要部分： 購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="004e1-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="004e1-116">訪客可以依內容類型瀏覽專輯、 將專輯新增至購物車、 檢閱其選取項目和最後位進行結帳，登入並完成訂購。</span><span class="sxs-lookup"><span data-stu-id="004e1-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="004e1-117">此外，儲存區系統管理員將能夠管理可用的相簿，以及其主要屬性。</span><span class="sxs-lookup"><span data-stu-id="004e1-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="004e1-118">![音樂市集畫面](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store 畫面")</span><span class="sxs-lookup"><span data-stu-id="004e1-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="004e1-119">*Music Store 畫面*</span><span class="sxs-lookup"><span data-stu-id="004e1-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="004e1-120">ASP.NET MVC 4 基本資訊</span><span class="sxs-lookup"><span data-stu-id="004e1-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="004e1-121">音樂市集 」 應用程式會使用來建置**模型檢視控制器 (MVC)**，分隔成三個主要元件的應用程式的架構模式：</span><span class="sxs-lookup"><span data-stu-id="004e1-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="004e1-122">**模型**： 模型物件屬於實作網域邏輯應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="004e1-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="004e1-123">通常，模型物件也擷取，並將模型狀態儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="004e1-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="004e1-124">**檢視：** 檢視是顯示應用程式的使用者介面 (UI) 的元件。</span><span class="sxs-lookup"><span data-stu-id="004e1-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="004e1-125">一般而言，此 UI 是從模型資料的方式建立。</span><span class="sxs-lookup"><span data-stu-id="004e1-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="004e1-126">範例會顯示文字方塊和依相簿物件的目前狀態的下拉式清單編輯檢視的專輯。</span><span class="sxs-lookup"><span data-stu-id="004e1-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="004e1-127">**控制站：** 控制器就是元件，以處理使用者互動、 操作模型，並最終選取要轉譯的 UI 的檢視。</span><span class="sxs-lookup"><span data-stu-id="004e1-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="004e1-128">在 MVC 應用程式中，檢視只會顯示資訊，而控制器則會處理及回應使用者輸入和互動。</span><span class="sxs-lookup"><span data-stu-id="004e1-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="004e1-129">MVC 模式可協助您建立不同的應用程式 （輸入的邏輯、 商務邏輯和 UI 邏輯），同時提供這些項目之間的鬆散結合的不同層面的應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="004e1-130">這種區隔可協助您管理複雜度，當您建置應用程式，因為它可讓您專注於一次實作的其中一個層面。</span><span class="sxs-lookup"><span data-stu-id="004e1-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="004e1-131">此外，MVC 模式可讓您輕鬆地測試應用程式，也鼓勵使用測試導向開發 (TDD)，來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="004e1-132">**ASP.NET MVC**架構建立 ASP.NET MVC 為基礎的 Web 應用程式提供的 ASP.NET Web Form 模式的替代方案。</span><span class="sxs-lookup"><span data-stu-id="004e1-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="004e1-133">**ASP.NET MVC** framework 是一種輕量型、 可高度測試的展示架構，（就像 Web form 架構應用程式） 整合了現有的 ASP.NET 功能，例如主版頁面和成員資格為基礎驗證讓您取得核心.NET framework 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="004e1-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="004e1-134">這非常有用，如果您已熟悉 ASP.NET Web Form 因為您已使用的所有程式庫也可使用 ASP.NET MVC 4 中。</span><span class="sxs-lookup"><span data-stu-id="004e1-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="004e1-135">此外，MVC 應用程式的三個主要元件之間的鬆散結合也會提升平行開發。</span><span class="sxs-lookup"><span data-stu-id="004e1-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="004e1-136">比方說，開發人員能夠檢視、 控制器邏輯，可以處理第二個開發人員和第三個開發人員可以專注於商務邏輯模型中。</span><span class="sxs-lookup"><span data-stu-id="004e1-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="004e1-137">目標</span><span class="sxs-lookup"><span data-stu-id="004e1-137">Objectives</span></span>

<span data-ttu-id="004e1-138">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="004e1-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="004e1-139">根據音樂市集 」 應用程式教學課程從頭開始建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="004e1-140">新增控制站來處理至站台和瀏覽其主要功能的首頁的 Url</span><span class="sxs-lookup"><span data-stu-id="004e1-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="004e1-141">加入檢視，以自訂其樣式以及顯示的內容</span><span class="sxs-lookup"><span data-stu-id="004e1-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="004e1-142">新增模型類別來包含和管理資料和網域邏輯</span><span class="sxs-lookup"><span data-stu-id="004e1-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="004e1-143">將從控制器動作的資訊傳遞至檢視範本中使用檢視模型模式</span><span class="sxs-lookup"><span data-stu-id="004e1-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="004e1-144">瀏覽 ASP.NET MVC 4 的新範本的網際網路應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="004e1-145">必要條件</span><span class="sxs-lookup"><span data-stu-id="004e1-145">Prerequisites</span></span>

<span data-ttu-id="004e1-146">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="004e1-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="004e1-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (讀取[附錄 A](#AppendixA)如需有關如何安裝)</span><span class="sxs-lookup"><span data-stu-id="004e1-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="004e1-148">安裝程式</span><span class="sxs-lookup"><span data-stu-id="004e1-148">Setup</span></span>

<span data-ttu-id="004e1-149">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="004e1-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="004e1-150">為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="004e1-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="004e1-151">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="004e1-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="004e1-152">如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。</span><span class="sxs-lookup"><span data-stu-id="004e1-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="004e1-153">練習</span><span class="sxs-lookup"><span data-stu-id="004e1-153">Exercises</span></span>

<span data-ttu-id="004e1-154">這個實際操作實驗室是由下列練習所組成的：</span><span class="sxs-lookup"><span data-stu-id="004e1-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="004e1-155">練習 1： 建立 MusicStore ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="004e1-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="004e1-156">練習 2： 建立控制器</span><span class="sxs-lookup"><span data-stu-id="004e1-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="004e1-157">練習 3： 將參數傳遞至控制器</span><span class="sxs-lookup"><span data-stu-id="004e1-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="004e1-158">練習 4： 建立檢視</span><span class="sxs-lookup"><span data-stu-id="004e1-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="004e1-159">練習 5： 建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="004e1-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="004e1-160">練習 6： 在檢視中使用參數</span><span class="sxs-lookup"><span data-stu-id="004e1-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="004e1-161">練習 7: 瀏覽 ASP.NET MVC 4 的新範本</span><span class="sxs-lookup"><span data-stu-id="004e1-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="004e1-162">每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="004e1-163">如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。</span><span class="sxs-lookup"><span data-stu-id="004e1-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="004e1-164">完成這個實驗室估計時間： **60 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="004e1-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="004e1-165">練習 1： 建立 MusicStore ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="004e1-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="004e1-166">在此練習中，您將學習如何在 Visual Studio 2012 Express for Web，以及其主要資料夾組織建立 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="004e1-167">此外，您將學習如何加入新的控制器，並讓它在應用程式的首頁上顯示一個簡單的字串。</span><span class="sxs-lookup"><span data-stu-id="004e1-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="004e1-168">工作 1-建立 ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="004e1-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="004e1-169">在這個工作中，您將建立空白 ASP.NET MVC 應用程式專案中使用 MVC 的 Visual Studio 範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="004e1-170">開始**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="004e1-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="004e1-171">按一下 [檔案] 功能表上的 [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="004e1-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="004e1-172">在 [**新的專案**] 對話方塊中選取**ASP.NET MVC 4 Web 應用程式**專案類型，位於**Visual C#** **Web**範本清單。</span><span class="sxs-lookup"><span data-stu-id="004e1-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="004e1-173">變更**名稱**要*MvcMusicStore*。</span><span class="sxs-lookup"><span data-stu-id="004e1-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="004e1-174">設定內的新方案的位置**開始**在這個練習中的來源資料夾中，如範例的資料夾 **[您-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**。</span><span class="sxs-lookup"><span data-stu-id="004e1-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="004e1-175">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="004e1-175">Click **OK**.</span></span>

    <span data-ttu-id="004e1-176">![建立新專案 對話方塊](aspnet-mvc-4-fundamentals/_static/image2.png "建立新專案 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="004e1-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="004e1-177">*建立新專案 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="004e1-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="004e1-178">在 [**新的 ASP.NET MVC 4 專案**] 對話方塊中選取**基本**範本並確定**檢視引擎**選取**Razor**。</span><span class="sxs-lookup"><span data-stu-id="004e1-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="004e1-179">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="004e1-179">Click **OK**.</span></span>

    <span data-ttu-id="004e1-180">![新 ASP.NET MVC 4 專案 對話方塊](aspnet-mvc-4-fundamentals/_static/image3.png "新 ASP.NET MVC 4 專案 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="004e1-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="004e1-181">*新 ASP.NET MVC 4 專案 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="004e1-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="004e1-182">工作 2-瀏覽方案結構</span><span class="sxs-lookup"><span data-stu-id="004e1-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="004e1-183">ASP.NET MVC 架構包括 Visual Studio 專案範本，可協助您建立支援 MVC 模式的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="004e1-184">此範本會建立新的 ASP.NET MVC Web 應用程式具有必要的資料夾、 項目範本和組態檔項目。</span><span class="sxs-lookup"><span data-stu-id="004e1-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="004e1-185">在這個工作中，您將檢查方案結構，以了解所涉及的項目和其關聯性。</span><span class="sxs-lookup"><span data-stu-id="004e1-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="004e1-186">下列資料夾會包含在所有 ASP.NET MVC 應用程式，因為預設的 ASP.NET MVC 架構會使用&quot;慣例優於組態&quot;方法，並讓一些預設假設根據資料夾名稱慣例。</span><span class="sxs-lookup"><span data-stu-id="004e1-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="004e1-187">一旦建立專案時，檢閱已在右側的 [方案總管] 中建立的資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="004e1-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="004e1-188">![在 [方案總管] 中的 ASP.NET MVC 資料夾結構](aspnet-mvc-4-fundamentals/_static/image4.png "方案總管 中的 ASP.NET MVC 資料夾結構")</span><span class="sxs-lookup"><span data-stu-id="004e1-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="004e1-189">*在 [方案總管] 中的 ASP.NET MVC 資料夾結構*</span><span class="sxs-lookup"><span data-stu-id="004e1-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="004e1-190">**控制站**。</span><span class="sxs-lookup"><span data-stu-id="004e1-190">**Controllers**.</span></span> <span data-ttu-id="004e1-191">這個資料夾將包含控制器類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="004e1-192">MVC 型應用程式中，控制器是負責處理使用者互動、 操作模型，以及最終選擇一種檢視來呈現 UI。</span><span class="sxs-lookup"><span data-stu-id="004e1-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="004e1-193">MVC framework 需要結尾的所有控制器的名稱&quot;控制器&quot;-例如 HomeController、 LoginController 或 ProductController。</span><span class="sxs-lookup"><span data-stu-id="004e1-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="004e1-194">**模型**。</span><span class="sxs-lookup"><span data-stu-id="004e1-194">**Models**.</span></span> <span data-ttu-id="004e1-195">代表 MVC Web 應用程式的應用程式模型會提供此資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="004e1-196">這通常包含定義物件和資料存放區與互動的邏輯的程式碼。</span><span class="sxs-lookup"><span data-stu-id="004e1-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="004e1-197">一般而言，實際的模型物件會在不同的類別庫中。</span><span class="sxs-lookup"><span data-stu-id="004e1-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="004e1-198">不過，當您建立新的應用程式時，您可能會包含類別，並再將它們移到不同的類別庫，在稍後的時間點，在開發週期。</span><span class="sxs-lookup"><span data-stu-id="004e1-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="004e1-199">**檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-199">**Views**.</span></span> <span data-ttu-id="004e1-200">此資料夾是建議放置檢視，負責顯示應用程式的使用者介面元件的位置。</span><span class="sxs-lookup"><span data-stu-id="004e1-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="004e1-201">檢視使用.aspx、.ascx、.cshtml 和.master 檔案，除了與呈現檢視相關的任何其他檔案。</span><span class="sxs-lookup"><span data-stu-id="004e1-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="004e1-202">Views 資料夾會包含每個控制站; 所需的資料夾控制器名稱的前置詞的命名的資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="004e1-203">例如，如果您有名稱為控制器**HomeController**，[Views] 資料夾會包含名為 Home 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="004e1-204">根據預設，當 ASP.NET MVC 架構載入檢視時，它會尋找要求的檢視名稱 Views\controllerName 資料夾中的.aspx 檔案 (**檢視 [ControllerName] [Action].aspx**) 或 (**檢視 [ControllerName][動作].cshtml**) 的 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="004e1-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="004e1-205">除了先前所列的資料夾，MVC Web 應用程式會使用**Global.asax**檔案來設定全域 URL 路由預設值，並使用**Web.config**檔案來設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="004e1-206">工作 3-新增 HomeController</span><span class="sxs-lookup"><span data-stu-id="004e1-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="004e1-207">在不使用 MVC framework 的 ASP.NET 應用程式，使用者互動會圍繞網頁以及引發和處理從這些網頁事件加以組織。</span><span class="sxs-lookup"><span data-stu-id="004e1-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="004e1-208">相反地，ASP.NET MVC 應用程式的使用者互動會圍繞控制器和動作方法加以組織。</span><span class="sxs-lookup"><span data-stu-id="004e1-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="004e1-209">相反地，ASP.NET MVC 架構會將 Url 對應至稱為控制器的類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="004e1-210">控制站處理連入要求、 處理使用者輸入和互動、 執行適當的應用程式邏輯並決定要傳送回用戶端的回應 （顯示 HTML、 下載檔案、 重新導向至不同的 URL，依此類推）。</span><span class="sxs-lookup"><span data-stu-id="004e1-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="004e1-211">在顯示 HTML，控制器類別通常會呼叫不同的檢視元件來產生要求的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="004e1-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="004e1-212">在 MVC 應用程式中，檢視只會顯示資訊，而控制器則會處理及回應使用者輸入和互動。</span><span class="sxs-lookup"><span data-stu-id="004e1-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="004e1-213">在這個工作中，您將加入的音樂市集 」 網站的 [首頁] 頁面來處理 Url 的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="004e1-214">以滑鼠右鍵按一下**控制器**在 [方案總管] 中選取的資料夾**新增**，然後**控制器**命令：</span><span class="sxs-lookup"><span data-stu-id="004e1-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="004e1-215">![新增控制器指令](aspnet-mvc-4-fundamentals/_static/image5.png "新增控制器命令")</span><span class="sxs-lookup"><span data-stu-id="004e1-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="004e1-216">*新增控制器 命令*</span><span class="sxs-lookup"><span data-stu-id="004e1-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="004e1-217">**新增控制器**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="004e1-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="004e1-218">控制器*HomeController*然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="004e1-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="004e1-219">![新增控制器 對話方塊](aspnet-mvc-4-fundamentals/_static/image6.png "新增控制器 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="004e1-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="004e1-220">*新增控制器 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="004e1-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="004e1-221">檔案**HomeController.cs**中建立**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="004e1-222">才能**HomeController**其索引的動作傳回的字串，取代**索引**為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="004e1-223">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex1 HomeController 索引*)</span><span class="sxs-lookup"><span data-stu-id="004e1-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="004e1-224">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="004e1-225">在這個工作中，您可以嘗試在網頁瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="004e1-226">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="004e1-227">編譯專案，並在本機 IIS Web 伺服器啟動。</span><span class="sxs-lookup"><span data-stu-id="004e1-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="004e1-228">本機 IIS Web 伺服器會自動開啟 web 瀏覽器並指向 Web 伺服器的 URL。</span><span class="sxs-lookup"><span data-stu-id="004e1-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="004e1-229">![在網頁瀏覽器中執行的應用程式](aspnet-mvc-4-fundamentals/_static/image7.png "在網頁瀏覽器中執行的應用程式")</span><span class="sxs-lookup"><span data-stu-id="004e1-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="004e1-230">*在網頁瀏覽器中執行的應用程式*</span><span class="sxs-lookup"><span data-stu-id="004e1-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-231">本機 IIS Web 伺服器會執行網站上的隨機的可用連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="004e1-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="004e1-232">在上圖中，站台執行在`http://localhost:50103/`，因此它使用連接埠 50103。</span><span class="sxs-lookup"><span data-stu-id="004e1-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="004e1-233">連接埠號碼可能有所不同。</span><span class="sxs-lookup"><span data-stu-id="004e1-233">Your port number may vary.</span></span>
2. <span data-ttu-id="004e1-234">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="004e1-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="004e1-235">練習 2： 建立控制器</span><span class="sxs-lookup"><span data-stu-id="004e1-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="004e1-236">在此練習中，您將學習如何控制站來實作簡單的音樂市集應用程式的功能更新。</span><span class="sxs-lookup"><span data-stu-id="004e1-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="004e1-237">該控制器會定義處理每個下列的特定要求的動作方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="004e1-238">在 Music Store 中的音樂內容類型的清單頁面</span><span class="sxs-lookup"><span data-stu-id="004e1-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="004e1-239">列出所有針對特定的內容類型的音樂 album 瀏覽 頁面</span><span class="sxs-lookup"><span data-stu-id="004e1-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="004e1-240">詳細資料頁面，其中顯示有關特定音樂專輯資訊</span><span class="sxs-lookup"><span data-stu-id="004e1-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="004e1-241">本練習的範圍，這些動作只會傳回字串，到目前為止。</span><span class="sxs-lookup"><span data-stu-id="004e1-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="004e1-242">工作 1-加入新的 StoreController 類別</span><span class="sxs-lookup"><span data-stu-id="004e1-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="004e1-243">在這個工作中，您將加入新的控制器。</span><span class="sxs-lookup"><span data-stu-id="004e1-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="004e1-244">如果尚未開啟，啟動**VS Express for Web 2012**。</span><span class="sxs-lookup"><span data-stu-id="004e1-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="004e1-245">在 **檔案**功能表上，選擇**開啟專案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="004e1-246">在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex02 CreatingAController\Begin**，選取**Begin.sln**然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="004e1-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="004e1-247">或者，您可以繼續使用解決方案取得完成前一個練習之後。</span><span class="sxs-lookup"><span data-stu-id="004e1-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="004e1-248">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="004e1-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="004e1-249">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="004e1-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="004e1-250">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="004e1-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="004e1-251">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="004e1-252">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="004e1-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="004e1-253">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="004e1-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="004e1-254">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="004e1-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="004e1-255">加入新的控制器。</span><span class="sxs-lookup"><span data-stu-id="004e1-255">Add the new controller.</span></span> <span data-ttu-id="004e1-256">若要這樣做，請以滑鼠右鍵按一下**控制器**在 [方案總管] 中選取的資料夾**新增**，然後**控制器**命令。</span><span class="sxs-lookup"><span data-stu-id="004e1-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="004e1-257">變更**控制器名稱**要*StoreController*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="004e1-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="004e1-258">![新增控制器 對話方塊](aspnet-mvc-4-fundamentals/_static/image8.png "新增控制器 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="004e1-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="004e1-259">*新增控制器 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="004e1-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="004e1-260">工作 2-修改 StoreController 的動作</span><span class="sxs-lookup"><span data-stu-id="004e1-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="004e1-261">在這個工作中，您將修改呼叫控制器方法**動作**。</span><span class="sxs-lookup"><span data-stu-id="004e1-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="004e1-262">動作是負責處理 URL 要求，並判斷應該傳送回瀏覽器或叫用的 URL 的使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="004e1-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="004e1-263">**StoreController**類別已經有**Index**方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="004e1-264">您將會稍後在本實驗室中使用它來實作，其中列出所有的內容類型的音樂市集頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="004e1-265">現在，只要取代**Index**方法，以下列程式碼會傳回字串&quot;Hello from Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="004e1-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="004e1-266">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex2 StoreController 索引*)</span><span class="sxs-lookup"><span data-stu-id="004e1-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="004e1-267">新增**瀏覽**並**詳細資料**方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="004e1-268">若要這樣做，請新增下列程式碼**StoreController**:</span><span class="sxs-lookup"><span data-stu-id="004e1-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="004e1-269">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="004e1-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="004e1-270">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="004e1-271">在這個工作中，您可以嘗試在網頁瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="004e1-272">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-273">在專案開始**首頁**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="004e1-274">變更 URL，以確認每個動作的實作。</span><span class="sxs-lookup"><span data-stu-id="004e1-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="004e1-275">**/ 儲存**。</span><span class="sxs-lookup"><span data-stu-id="004e1-275">**/Store**.</span></span> <span data-ttu-id="004e1-276">您會看到 **&quot;Hello from Store.Index()&quot;**。</span><span class="sxs-lookup"><span data-stu-id="004e1-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="004e1-277">**/ Store/瀏覽**。</span><span class="sxs-lookup"><span data-stu-id="004e1-277">**/Store/Browse**.</span></span> <span data-ttu-id="004e1-278">您會看到 **&quot;Hello from Store.Browse()&quot;**。</span><span class="sxs-lookup"><span data-stu-id="004e1-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="004e1-279">**/ Store/Details**。</span><span class="sxs-lookup"><span data-stu-id="004e1-279">**/Store/Details**.</span></span> <span data-ttu-id="004e1-280">您會看到 **&quot;Hello from Store.Details()&quot;**。</span><span class="sxs-lookup"><span data-stu-id="004e1-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="004e1-281">![瀏覽 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "瀏覽 StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="004e1-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="004e1-282">*瀏覽 /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="004e1-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="004e1-283">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="004e1-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="004e1-284">練習 3： 將參數傳遞至控制器</span><span class="sxs-lookup"><span data-stu-id="004e1-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="004e1-285">到目前為止，您有已傳回常數字串中的控制器。</span><span class="sxs-lookup"><span data-stu-id="004e1-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="004e1-286">在這個練習中，您將學習如何將參數傳遞至控制站使用的 URL 和查詢字串，，然後以文字回應到瀏覽器的方法動作。</span><span class="sxs-lookup"><span data-stu-id="004e1-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="004e1-287">工作 1-StoreController 新增內容類型參數</span><span class="sxs-lookup"><span data-stu-id="004e1-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="004e1-288">在這個工作中，您將使用**querystring**傳送至的參數**瀏覽**中的動作方法**StoreController**。</span><span class="sxs-lookup"><span data-stu-id="004e1-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="004e1-289">如果尚未開啟，啟動**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="004e1-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="004e1-290">在 **檔案**功能表上，選擇**開啟專案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="004e1-291">在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex03 PassingParametersToAController\Begin**，選取**Begin.sln**然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="004e1-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="004e1-292">或者，您可以繼續使用解決方案取得完成前一個練習之後。</span><span class="sxs-lookup"><span data-stu-id="004e1-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="004e1-293">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="004e1-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="004e1-294">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="004e1-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="004e1-295">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="004e1-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="004e1-296">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="004e1-297">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="004e1-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="004e1-298">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="004e1-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="004e1-299">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="004e1-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="004e1-300">開啟**StoreController**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-300">Open **StoreController** class.</span></span> <span data-ttu-id="004e1-301">若要這樣做，請在**方案總管**，展開**控制站**資料夾，然後按兩下**StoreController.cs**。</span><span class="sxs-lookup"><span data-stu-id="004e1-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="004e1-302">變更**瀏覽**方法，將字串參數來要求特定的內容類型。</span><span class="sxs-lookup"><span data-stu-id="004e1-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="004e1-303">ASP.NET MVC 會自動傳遞任何查詢字串或表單張貼參數，叫做**genre**叫用時，此動作方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="004e1-304">若要這樣做，請取代**瀏覽**為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="004e1-305">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="004e1-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="004e1-306">您使用**HttpUtility.HtmlEncode**公用程式方法，以防止使用者插入檢視中的 Javascript，例如連結  **/Store/瀏覽？內容類型 =&lt;指令碼&gt;window.location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**。</span><span class="sxs-lookup"><span data-stu-id="004e1-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="004e1-307">如需進一步說明，請瀏覽[這篇 msdn 文章](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)。</span><span class="sxs-lookup"><span data-stu-id="004e1-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="004e1-308">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="004e1-309">在這個工作中，您將會嘗試在網頁瀏覽器應用程式，並使用**genre**參數。</span><span class="sxs-lookup"><span data-stu-id="004e1-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="004e1-310">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-311">在專案開始**首頁**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="004e1-312">將 URL 變更為  */儲存] / [瀏覽？內容類型 = Disco*以確認該動作所接收的內容類型參數。</span><span class="sxs-lookup"><span data-stu-id="004e1-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="004e1-313">![瀏覽 StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "瀏覽 StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="004e1-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="004e1-314">*瀏覽 /Store/Browse 嗎？內容類型 = Disco*</span><span class="sxs-lookup"><span data-stu-id="004e1-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="004e1-315">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="004e1-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="004e1-316">工作 3-加入 URL 中內嵌的 Id 參數</span><span class="sxs-lookup"><span data-stu-id="004e1-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="004e1-317">在這個工作中，您將使用**URL**傳遞**識別碼**參數**詳細資料**動作方法的**StoreController**。</span><span class="sxs-lookup"><span data-stu-id="004e1-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="004e1-318">ASP.NET MVC 的預設路由的慣例是將 URL 區段在動作方法的名稱之後，名為的參數視為**識別碼**。如果動作方法具有名為 Id 參數，然後 ASP.NET MVC 會自動傳遞的 URL 區段您做為參數。</span><span class="sxs-lookup"><span data-stu-id="004e1-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="004e1-319">在 URL 中**存放區/詳細資料/5**，**識別碼**會解譯為**5**。</span><span class="sxs-lookup"><span data-stu-id="004e1-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="004e1-320">變更**詳細資料**方法**StoreController**，來加入**int**稱為參數**識別碼**。若要這樣做，請取代**詳細資料**為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="004e1-321">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="004e1-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="004e1-322">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="004e1-323">在這個工作中，您將會嘗試在網頁瀏覽器應用程式，並使用**識別碼**參數。</span><span class="sxs-lookup"><span data-stu-id="004e1-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="004e1-324">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-325">在專案開始**首頁**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="004e1-326">將 URL 變更為 */Store/Details/5*以確認該動作所接收的 id 參數。</span><span class="sxs-lookup"><span data-stu-id="004e1-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="004e1-327">![瀏覽 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "瀏覽 StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="004e1-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="004e1-328">*瀏覽 /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="004e1-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="004e1-329">練習 4： 建立檢視</span><span class="sxs-lookup"><span data-stu-id="004e1-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="004e1-330">到目前為止您有已傳回字串從控制器動作。</span><span class="sxs-lookup"><span data-stu-id="004e1-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="004e1-331">雖然這十分有用的方式了解控制器的運作方式，是不實際的 Web 應用程式如何建立。</span><span class="sxs-lookup"><span data-stu-id="004e1-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="004e1-332">檢視是提供更好的方法來產生 HTML，回到瀏覽器中使用的範本檔案使用的元件。</span><span class="sxs-lookup"><span data-stu-id="004e1-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="004e1-333">在這個練習中，您將了解如何新增版面配置主版頁面，設定一般的 HTML 內容，來增強網站以及最後的檢視範本，以啟用傳回 HTML HomeController 外觀的樣式表的範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="004e1-334">工作 1-修改檔案\_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="004e1-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="004e1-335">檔案 **~/Views/Shared/\_layout.cshtml**可讓您設定整個網站上使用的通用 HTML 的範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="004e1-336">在這項工作中，要將配置主版頁面的連結的通用標頭加入 首頁 存放區 區域中。</span><span class="sxs-lookup"><span data-stu-id="004e1-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="004e1-337">如果尚未開啟，啟動**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="004e1-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="004e1-338">在 **檔案**功能表上，選擇**開啟專案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="004e1-339">在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex04 CreatingAView\Begin**，選取**Begin.sln**然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="004e1-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="004e1-340">或者，您可以繼續使用解決方案取得完成前一個練習之後。</span><span class="sxs-lookup"><span data-stu-id="004e1-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="004e1-341">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="004e1-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="004e1-342">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="004e1-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="004e1-343">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="004e1-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="004e1-344">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="004e1-345">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="004e1-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="004e1-346">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="004e1-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="004e1-347">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="004e1-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="004e1-348">檔案 <strong>\_layout.cshtml</strong>包含站台上的所有頁面的 HTML 容器配置。</span><span class="sxs-lookup"><span data-stu-id="004e1-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="004e1-349">它包含<strong>&lt;html&gt;</strong> HTML 回應中，項目，以及<strong>&lt;head&gt;</strong>並<strong>&lt;主體&gt;</strong>項目。</span><span class="sxs-lookup"><span data-stu-id="004e1-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="004e1-350"><strong>@RenderBody（)</strong>內 HTML 主體識別區域該範本可以填入 動態內容的檢視。</span><span class="sxs-lookup"><span data-stu-id="004e1-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="004e1-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="004e1-352">新增具有連結的通用標頭中的站台的所有頁面的 [首頁和存放區] 區域。</span><span class="sxs-lookup"><span data-stu-id="004e1-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="004e1-353">若要這樣做，請新增下列程式碼下方&lt;主體&gt;陳述式。</span><span class="sxs-lookup"><span data-stu-id="004e1-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="004e1-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="004e1-355">包含要呈現的每個頁面的主體區段的 div。</span><span class="sxs-lookup"><span data-stu-id="004e1-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="004e1-356">取代 <strong>@RenderBody（)</strong>下列 higlighted 程式碼: (C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="004e1-357">您知道嗎？</span><span class="sxs-lookup"><span data-stu-id="004e1-357">Did you know?</span></span> <span data-ttu-id="004e1-358">Visual Studio 2012 有程式碼片段，讓您輕鬆將常用的程式碼加入 HTML、 程式碼檔案和更多功能 ！</span><span class="sxs-lookup"><span data-stu-id="004e1-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="004e1-359">請試著輸入**&lt;div&gt;** 並按下**索引標籤**兩次來插入完整**div**標記。</span><span class="sxs-lookup"><span data-stu-id="004e1-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="004e1-360">工作 2-新增的 CSS 樣式表</span><span class="sxs-lookup"><span data-stu-id="004e1-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="004e1-361">空白專案範本包含非常簡化的 CSS 檔案只包含用來顯示基本的表單和驗證訊息的樣式。</span><span class="sxs-lookup"><span data-stu-id="004e1-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="004e1-362">您將使用其他的 CSS 和映像 （可能是由設計工具提供），以加強網站的外觀與風格。</span><span class="sxs-lookup"><span data-stu-id="004e1-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="004e1-363">在這個工作中，您將加入的 CSS 樣式表，來定義網站的樣式。</span><span class="sxs-lookup"><span data-stu-id="004e1-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="004e1-364">CSS 檔案和映像會包含在**Source\Assets\Content**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="004e1-365">若要將它們新增至應用程式中，拖曳其內容從**Windows 檔案總管**到視窗**方案總管 中**在 Visual Studio Express for Web，如下所示：</span><span class="sxs-lookup"><span data-stu-id="004e1-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="004e1-366">![拖曳樣式內容](aspnet-mvc-4-fundamentals/_static/image12.png "拖曳樣式內容")</span><span class="sxs-lookup"><span data-stu-id="004e1-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="004e1-367">*拖曳樣式內容*</span><span class="sxs-lookup"><span data-stu-id="004e1-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="004e1-368">警告會出現一個對話方塊，確認是否要取代詢問**Site.css**檔案和一些現有的映像。</span><span class="sxs-lookup"><span data-stu-id="004e1-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="004e1-369">請檢查**套用至所有項目**然後按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="004e1-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="004e1-370">工作 3-新增檢視範本</span><span class="sxs-lookup"><span data-stu-id="004e1-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="004e1-371">在這個工作中，您將檢視範本來產生將使用的版面配置的主版頁面的 HTML 回應，並在這個練習中，新增 CSS。</span><span class="sxs-lookup"><span data-stu-id="004e1-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="004e1-372">若要瀏覽網站的首頁時，請使用檢視範本，您首先需要而不是傳回字串，表示**HomeController 索引**方法會傳回**檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="004e1-373">開啟**HomeController**類別，並變更其**Index**方法，以傳回**ActionResult**，並且會傳回**View()**。</span><span class="sxs-lookup"><span data-stu-id="004e1-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="004e1-374">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex4 HomeController 索引*)</span><span class="sxs-lookup"><span data-stu-id="004e1-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="004e1-375">現在，您需要新增適當的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="004e1-376">若要這樣做，請**上按一下滑鼠右鍵**內**Index**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="004e1-377">此時會出現**加入檢視**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="004e1-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="004e1-378">![加入從索引方法內的檢視](aspnet-mvc-4-fundamentals/_static/image13.png "加入從索引方法內的檢視")</span><span class="sxs-lookup"><span data-stu-id="004e1-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="004e1-379">*加入從索引方法內的檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="004e1-380">**加入檢視**對話方塊隨即出現，產生的檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="004e1-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="004e1-381">根據預設，此對話方塊會預先填入檢視範本的名稱，使其符合使用它的動作方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="004e1-382">因為您使用**加入檢視**內的操作功能表**索引**HomeController，動作方法**加入檢視**對話方塊具有做為預設檢視名稱的索引。</span><span class="sxs-lookup"><span data-stu-id="004e1-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="004e1-383">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="004e1-383">Click **Add**.</span></span>

    <span data-ttu-id="004e1-384">![新增檢視 對話方塊](aspnet-mvc-4-fundamentals/_static/image14.png "新增檢視對話方塊")</span><span class="sxs-lookup"><span data-stu-id="004e1-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="004e1-385">*新增檢視對話方塊*</span><span class="sxs-lookup"><span data-stu-id="004e1-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="004e1-386">Visual Studio 會產生**Index.cshtml**內的檢視範本**Views\Home**資料夾，然後開啟它。</span><span class="sxs-lookup"><span data-stu-id="004e1-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="004e1-387">![首頁建立的索引檢視](aspnet-mvc-4-fundamentals/_static/image15.png "建立首頁索引檢視")</span><span class="sxs-lookup"><span data-stu-id="004e1-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="004e1-388">*建立主索引檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-389">名稱和位置**Index.cshtml**檔案與相關，而且依照預設 ASP.NET MVC 的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="004e1-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="004e1-390">資料夾 \Views\**首頁** 符合控制器名稱 (**首頁**控制器)。</span><span class="sxs-lookup"><span data-stu-id="004e1-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="004e1-391">檢視範本名稱 (**Index**)，符合將會顯示檢視控制器動作方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="004e1-392">如此一來，ASP.NET MVC，就不需要使用此命名慣例，傳回的檢視時，明確指定的名稱或檢視範本的位置。</span><span class="sxs-lookup"><span data-stu-id="004e1-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="004e1-393">產生的檢視範本為基礎 **\_layout.cshtml**稍早定義的範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="004e1-394">要更新 ViewBag.Title 屬性**首頁**，並變更到主要內容**這是首頁**，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="004e1-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="004e1-395">選取  **MvcMusicStore**專案，在 方案總管，然後按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="004e1-396">工作 4： 驗證</span><span class="sxs-lookup"><span data-stu-id="004e1-396">Task 4: Verification</span></span>

<span data-ttu-id="004e1-397">若要確認，您已在前一個練習中，正確地執行所有步驟，請繼續進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="004e1-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="004e1-398">在瀏覽器中開啟應用程式時，您應該注意到：</span><span class="sxs-lookup"><span data-stu-id="004e1-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="004e1-399">HomeController 的索引動作方法找到並顯示**\Views\Home\Index.cshtml**檢視範本，即使呼叫的程式碼**傳回 View()**，因為檢視範本，然後再標準的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="004e1-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="004e1-400">首頁會顯示歡迎訊息內定義**\Views\Home\Index.cshtml**檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="004e1-401">使用首頁 **\_layout.cshtml**範本，因此歡迎訊息內含 HTML 的標準網站的版面配置。</span><span class="sxs-lookup"><span data-stu-id="004e1-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="004e1-402">![首頁使用的定義 LayoutPage 和樣式的索引檢視](aspnet-mvc-4-fundamentals/_static/image16.png "首頁索引檢視，使用定義 LayoutPage 和樣式")</span><span class="sxs-lookup"><span data-stu-id="004e1-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="004e1-403">*首頁使用的定義 LayoutPage 和樣式的索引檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="004e1-404">練習 5： 建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="004e1-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="004e1-405">到目前為止，進行您的檢視顯示硬式編碼的 HTML，但若要建立動態 web 應用程式，檢視範本應該從控制器接收資訊。</span><span class="sxs-lookup"><span data-stu-id="004e1-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="004e1-406">要用於此用途的一個常用的技巧是**ViewModel**模式，可讓控制器能夠封裝來產生適當的 HTML 回應所需的所有資訊。</span><span class="sxs-lookup"><span data-stu-id="004e1-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="004e1-407">在此練習中，您將了解如何建立 ViewModel 類別，並新增必要的屬性： 的存放區，這些內容類型清單中的內容類型的數目。</span><span class="sxs-lookup"><span data-stu-id="004e1-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="004e1-408">您也會更新以使用建立的 ViewModel，StoreController，最後，您將在其中建立新的檢視範本將會顯示在頁面中所述的屬性。</span><span class="sxs-lookup"><span data-stu-id="004e1-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="004e1-409">工作 1-建立 ViewModel 類別</span><span class="sxs-lookup"><span data-stu-id="004e1-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="004e1-410">在這個工作中，您將建立會實作存放區的內容類型清單案例的 ViewModel 類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="004e1-411">如果尚未開啟，啟動**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="004e1-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="004e1-412">在 **檔案**功能表上，選擇**開啟專案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="004e1-413">在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex05 CreatingAViewModel\Begin**，選取**Begin.sln**然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="004e1-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="004e1-414">或者，您可以繼續使用解決方案取得完成前一個練習之後。</span><span class="sxs-lookup"><span data-stu-id="004e1-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="004e1-415">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="004e1-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="004e1-416">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="004e1-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="004e1-417">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="004e1-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="004e1-418">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="004e1-419">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="004e1-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="004e1-420">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="004e1-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="004e1-421">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="004e1-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="004e1-422">建立**Viewmodel**資料夾以保存 ViewModel。</span><span class="sxs-lookup"><span data-stu-id="004e1-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="004e1-423">若要這樣做，以滑鼠右鍵按一下最上層**MvcMusicStore**專案，然後選取**新增**，然後**新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="004e1-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="004e1-424">![加入新的資料夾](aspnet-mvc-4-fundamentals/_static/image17.png "新增新的資料夾")</span><span class="sxs-lookup"><span data-stu-id="004e1-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="004e1-425">*加入新的資料夾*</span><span class="sxs-lookup"><span data-stu-id="004e1-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="004e1-426">將資料夾命名*Viewmodel*。</span><span class="sxs-lookup"><span data-stu-id="004e1-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="004e1-427">![在 [方案總管] 中的 Viewmodel 資料夾](aspnet-mvc-4-fundamentals/_static/image18.png "方案總管 中的 Viewmodel 資料夾")</span><span class="sxs-lookup"><span data-stu-id="004e1-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="004e1-428">*在 [方案總管] 中的 Viewmodel 資料夾*</span><span class="sxs-lookup"><span data-stu-id="004e1-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="004e1-429">建立**ViewModel**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="004e1-430">若要這樣做，以滑鼠右鍵按一下**Viewmodel**最近建立資料夾，選取**新增**，然後**新項目**。</span><span class="sxs-lookup"><span data-stu-id="004e1-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="004e1-431">底下**程式碼**，選擇**類別**項目，然後將檔案命名為*StoreIndexViewModel.cs*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="004e1-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="004e1-432">![加入新的類別](aspnet-mvc-4-fundamentals/_static/image19.png "加入新的類別")</span><span class="sxs-lookup"><span data-stu-id="004e1-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="004e1-433">*加入新的類別*</span><span class="sxs-lookup"><span data-stu-id="004e1-433">*Adding a new Class*</span></span>

    <span data-ttu-id="004e1-434">![建立 StoreIndexViewModel 類別](aspnet-mvc-4-fundamentals/_static/image20.png "建立 StoreIndexViewModel 類別")</span><span class="sxs-lookup"><span data-stu-id="004e1-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="004e1-435">*建立 StoreIndexViewModel 類別*</span><span class="sxs-lookup"><span data-stu-id="004e1-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="004e1-436">工作 2-將屬性新增到 ViewModel 類別</span><span class="sxs-lookup"><span data-stu-id="004e1-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="004e1-437">有兩個參數，傳遞從 StoreController 至檢視範本以產生預期的 HTML 回應： 的存放區，這些內容類型清單中的內容類型的數目。</span><span class="sxs-lookup"><span data-stu-id="004e1-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="004e1-438">在這個工作中，您將這些 2 將屬性新增至**StoreIndexViewModel**類別： **NumberOfGenres** （整數） 和**內容類型**（字串的清單）。</span><span class="sxs-lookup"><span data-stu-id="004e1-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="004e1-439">新增**NumberOfGenres**並**內容類型**屬性，以**StoreIndexViewModel**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="004e1-440">若要這樣做，請在類別定義中加入下列 2 個幾行：</span><span class="sxs-lookup"><span data-stu-id="004e1-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="004e1-441">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex5 StoreIndexViewModel 屬性*)</span><span class="sxs-lookup"><span data-stu-id="004e1-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="004e1-442">**{Get; 設定;}** 標記法會利用 C# 的自動實作屬性 」 功能。</span><span class="sxs-lookup"><span data-stu-id="004e1-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="004e1-443">它提供屬性的優點，而不需要我們宣告支援欄位。</span><span class="sxs-lookup"><span data-stu-id="004e1-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="004e1-444">工作 3-使用 StoreIndexViewModel 更新 StoreController</span><span class="sxs-lookup"><span data-stu-id="004e1-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="004e1-445">**StoreIndexViewModel**類別會封裝將從所需的資訊**StoreController**的**索引**檢視範本，以便產生回應的方法.</span><span class="sxs-lookup"><span data-stu-id="004e1-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="004e1-446">在這個工作中，您將會更新**StoreController**使用**StoreIndexViewModel**。</span><span class="sxs-lookup"><span data-stu-id="004e1-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="004e1-447">開啟**StoreController**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="004e1-448">![開啟 StoreController 類別](aspnet-mvc-4-fundamentals/_static/image21.png "開啟 StoreController 類別")</span><span class="sxs-lookup"><span data-stu-id="004e1-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="004e1-449">*開啟 StoreController 類別*</span><span class="sxs-lookup"><span data-stu-id="004e1-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="004e1-450">若要使用**StoreIndexViewModel**從類別**StoreController**，在頂端加入下列命名空間**StoreController**程式碼：</span><span class="sxs-lookup"><span data-stu-id="004e1-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="004e1-451">(程式碼片段- *ASP.NET MVC 4 基本概念-使用 Viewmodel Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="004e1-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="004e1-452">變更**StoreController**的**Index**動作方法，讓它建立並填入**StoreIndexViewModel**物件，並再將它，傳遞至檢視範本產生 HTML 回應使用它。</span><span class="sxs-lookup"><span data-stu-id="004e1-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-453">在實驗室&quot;ASP.NET MVC 模型和資料存取&quot;您將撰寫程式碼，從資料庫擷取存放區的內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="004e1-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="004e1-454">在下列程式碼中，您將建立**清單**假資料內容類型，將會填入**StoreIndexViewModel**。</span><span class="sxs-lookup"><span data-stu-id="004e1-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="004e1-455">建立及設定後**StoreIndexViewModel**物件，它會當做引數傳遞給**檢視**方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="004e1-456">這表示在檢視範本會使用該物件來產生 HTML 回應使用它。</span><span class="sxs-lookup"><span data-stu-id="004e1-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="004e1-457">取代**Index**為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="004e1-458">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex5 StoreController Index 方法*)</span><span class="sxs-lookup"><span data-stu-id="004e1-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="004e1-459">如果您不熟悉使用 C#，您可能會假設使用**var**表示**viewModel**變數是晚期繫結。</span><span class="sxs-lookup"><span data-stu-id="004e1-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="004e1-460">不正確的-C# 編譯器判斷使用根據您指派給變數的型別推斷**viewModel**別的**StoreIndexViewModel**。</span><span class="sxs-lookup"><span data-stu-id="004e1-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="004e1-461">此外，藉由編譯本機**viewModel**變數設定為**StoreIndexViewModel** get 編譯時期檢查和 Visual Studio 程式碼編輯器支援類型。</span><span class="sxs-lookup"><span data-stu-id="004e1-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="004e1-462">工作 4-建立使用 StoreIndexViewModel 的檢視範本</span><span class="sxs-lookup"><span data-stu-id="004e1-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="004e1-463">在這個工作中，您將建立會使用從控制器傳遞 StoreIndexViewModel 物件顯示的內容類型清單的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="004e1-464">建立新的檢視範本之前, 我們將建置專案，讓**新增檢視 對話方塊**知道**StoreIndexViewModel**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="004e1-465">建置專案，方法是選取**建置**功能表項目，然後**建置 MvcMusicStore**。</span><span class="sxs-lookup"><span data-stu-id="004e1-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="004e1-466">![建置專案](aspnet-mvc-4-fundamentals/_static/image22.png "建置專案")</span><span class="sxs-lookup"><span data-stu-id="004e1-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="004e1-467">*建置專案*</span><span class="sxs-lookup"><span data-stu-id="004e1-467">*Building the project*</span></span>
2. <span data-ttu-id="004e1-468">建立新的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-468">Create a new View template.</span></span> <span data-ttu-id="004e1-469">若要這樣做，請以滑鼠右鍵按一下**Index**方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="004e1-470">![新增檢視](aspnet-mvc-4-fundamentals/_static/image23.png "新增檢視")</span><span class="sxs-lookup"><span data-stu-id="004e1-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="004e1-471">*新增檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-471">*Adding a View*</span></span>
3. <span data-ttu-id="004e1-472">因為**新增檢視 對話方塊**從叫用**StoreController**，它會將檢視範本新增預設會在**\Views\Store\Index.cshtml**檔案。</span><span class="sxs-lookup"><span data-stu-id="004e1-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="004e1-473">請檢查**建立強型別-檢視**核取方塊，然後選取**StoreIndexViewModel**作為**模型類別**。</span><span class="sxs-lookup"><span data-stu-id="004e1-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="004e1-474">此外，請確定選取的檢視引擎**Razor**。</span><span class="sxs-lookup"><span data-stu-id="004e1-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="004e1-475">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="004e1-475">Click **Add**.</span></span>

    <span data-ttu-id="004e1-476">![新增檢視 對話方塊](aspnet-mvc-4-fundamentals/_static/image24.png "新增檢視對話方塊")</span><span class="sxs-lookup"><span data-stu-id="004e1-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="004e1-477">*新增檢視對話方塊*</span><span class="sxs-lookup"><span data-stu-id="004e1-477">*Add View Dialog*</span></span>

    <span data-ttu-id="004e1-478">**\Views\Store\Index.cshtml**檢視範本檔案會建立並開啟。</span><span class="sxs-lookup"><span data-stu-id="004e1-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="004e1-479">根據提供的資訊**加入檢視**對話方塊，在最後一個步驟中，檢視範本會預期**StoreIndexViewModel**做為要用來產生 HTML 回應資料的執行個體。</span><span class="sxs-lookup"><span data-stu-id="004e1-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="004e1-480">您會發現範本會繼承`ViewPage<musicstore.viewmodels.storeindexviewmodel>`C# 中。</span><span class="sxs-lookup"><span data-stu-id="004e1-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="004e1-481">工作 5-正在更新檢視範本</span><span class="sxs-lookup"><span data-stu-id="004e1-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="004e1-482">在這個工作中，您將會更新擷取的內容類型和其名稱中的頁面數目在最後一個工作中建立的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="004e1-483">您將會使用語法 @ (通常稱為&quot;程式碼區塊&quot;) 來執行程式碼中檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="004e1-484">在  **Index.cshtml**檔案，內**存放區**資料夾中，其程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="004e1-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="004e1-485">透過內容類型 清單中的迴圈**StoreIndexViewModel**並建立 HTML **&lt;ul&gt;** 列出使用**foreach**迴圈。</span><span class="sxs-lookup"><span data-stu-id="004e1-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="004e1-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="004e1-487">按下**F5**以執行應用程式，並瀏覽 **/儲存**。</span><span class="sxs-lookup"><span data-stu-id="004e1-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="004e1-488">您會看到傳入的內容類型清單**StoreIndexViewModel**物件**StoreController**檢視範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="004e1-489">![顯示的內容類型清單的檢視](aspnet-mvc-4-fundamentals/_static/image26.png "檢視顯示的內容類型清單")</span><span class="sxs-lookup"><span data-stu-id="004e1-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="004e1-490">*顯示的內容類型清單的檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="004e1-491">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="004e1-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="004e1-492">練習 6： 在檢視中使用參數</span><span class="sxs-lookup"><span data-stu-id="004e1-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="004e1-493">練習 3 中您已了解如何將參數傳遞到控制器。</span><span class="sxs-lookup"><span data-stu-id="004e1-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="004e1-494">在此練習中，您將學習如何在檢視範本中使用這些參數。</span><span class="sxs-lookup"><span data-stu-id="004e1-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="004e1-495">基於這個目的，您將會介紹第一次，以協助您管理您的資料和網域邏輯的模型類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="004e1-496">此外，您將了解如何建立 ASP.NET MVC 應用程式內頁面的連結，而不必擔心的編碼方式的 URL 路徑等項目。</span><span class="sxs-lookup"><span data-stu-id="004e1-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="004e1-497">工作 1-新增模型類別</span><span class="sxs-lookup"><span data-stu-id="004e1-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="004e1-498">不同於 Viewmodel，只是為了將資訊從控制器傳遞至檢視建立模型類別是建置來包含和管理資料和網域邏輯。</span><span class="sxs-lookup"><span data-stu-id="004e1-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="004e1-499">在這項工作中，您會將兩個模型類別，來代表這些概念： **Genre**並**專輯**。</span><span class="sxs-lookup"><span data-stu-id="004e1-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="004e1-500">如果尚未開啟，啟動**VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="004e1-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="004e1-501">在 **檔案**功能表上，選擇**開啟專案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="004e1-502">在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex06 UsingParametersInView\Begin**，選取**Begin.sln**然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="004e1-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="004e1-503">或者，您可以繼續使用解決方案取得完成前一個練習之後。</span><span class="sxs-lookup"><span data-stu-id="004e1-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="004e1-504">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="004e1-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="004e1-505">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="004e1-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="004e1-506">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="004e1-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="004e1-507">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="004e1-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="004e1-508">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="004e1-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="004e1-509">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="004e1-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="004e1-510">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="004e1-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="004e1-511">新增**Genre**模型類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="004e1-512">若要這樣做，請以滑鼠右鍵按一下**模型**中的資料夾**方案總管**，選取**新增**，然後**新項目**選項。</span><span class="sxs-lookup"><span data-stu-id="004e1-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="004e1-513">底下**程式碼**，選擇**類別**項目，然後將檔案命名為*Genre.cs*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="004e1-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="004e1-514">![加入類別](aspnet-mvc-4-fundamentals/_static/image27.png "加入類別")</span><span class="sxs-lookup"><span data-stu-id="004e1-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="004e1-515">*加入新項目*</span><span class="sxs-lookup"><span data-stu-id="004e1-515">*Adding a new item*</span></span>

    <span data-ttu-id="004e1-516">![新增內容類型的模型類別](aspnet-mvc-4-fundamentals/_static/image28.png "新增內容類型的模型類別")</span><span class="sxs-lookup"><span data-stu-id="004e1-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="004e1-517">*新增內容類型的模型類別*</span><span class="sxs-lookup"><span data-stu-id="004e1-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="004e1-518">新增**名稱**內容類型類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="004e1-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="004e1-519">若要這樣做，請新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="004e1-519">To do this, add the following code:</span></span>

    <span data-ttu-id="004e1-520">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="004e1-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="004e1-521">後方的相同程序之前，新增**專輯**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="004e1-522">若要這樣做，請以滑鼠右鍵按一下**模型**中的資料夾**方案總管**，選取**新增**，然後**新項目**選項。</span><span class="sxs-lookup"><span data-stu-id="004e1-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="004e1-523">底下**程式碼**，選擇**類別**項目，然後將檔案命名為*Album.cs*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="004e1-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="004e1-524">專輯類別中加入兩個屬性： **Genre**並**標題**。</span><span class="sxs-lookup"><span data-stu-id="004e1-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="004e1-525">若要這樣做，請新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="004e1-525">To do this, add the following code:</span></span>

    <span data-ttu-id="004e1-526">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 專輯*)</span><span class="sxs-lookup"><span data-stu-id="004e1-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="004e1-527">工作 2-新增 StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="004e1-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="004e1-528">A **StoreBrowseViewModel**將這項工作中用來顯示符合所選的內容類型的 Album。</span><span class="sxs-lookup"><span data-stu-id="004e1-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="004e1-529">在這個工作中，您會建立這個類別，然後再新增 兩個屬性，以處理**Genre**及其**專輯**的清單。</span><span class="sxs-lookup"><span data-stu-id="004e1-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="004e1-530">新增**StoreBrowseViewModel**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="004e1-531">若要這樣做，請以滑鼠右鍵按一下**Viewmodel**中的資料夾**方案總管**，選取**新增**，然後**新項目**選項。</span><span class="sxs-lookup"><span data-stu-id="004e1-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="004e1-532">底下**程式碼**，選擇**類別**項目，然後將檔案命名為*StoreBrowseViewModel.cs*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="004e1-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="004e1-533">加入的參考中的模型**StoreBrowseViewModel**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="004e1-534">若要這樣做，請新增下列使用命名空間：</span><span class="sxs-lookup"><span data-stu-id="004e1-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="004e1-535">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="004e1-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="004e1-536">新增兩個屬性，以**StoreBrowseViewModel**類別： **Genre**並**專輯**。</span><span class="sxs-lookup"><span data-stu-id="004e1-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="004e1-537">若要這樣做，請新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="004e1-537">To do this, add the following code:</span></span>

    <span data-ttu-id="004e1-538">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="004e1-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="004e1-539">何謂**清單&lt;專輯&gt;** ？: 使用此定義**清單&lt;T&gt;** 型別，其中**T**限制哪些項目，這個型別**清單**屬於，在此情況下**專輯**（或其任何子系）。</span><span class="sxs-lookup"><span data-stu-id="004e1-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="004e1-540">這項功能設計類別和方法，延後的一或多個型別規格，直到類別或方法宣告和具現化用戶端程式碼是 C# 語言的一項功能稱為**泛型**。</span><span class="sxs-lookup"><span data-stu-id="004e1-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="004e1-541">**清單&lt;T&gt;** 相當於泛型**ArrayList**類型，適用於**System.Collections.Generic**命名空間。</span><span class="sxs-lookup"><span data-stu-id="004e1-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="004e1-542">其中一個使用的優點**泛型**是，由於指定型別，因此您不需要負責檢查作業，例如轉換到的項目型別的**專輯**和一樣具有**ArrayList**。</span><span class="sxs-lookup"><span data-stu-id="004e1-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="004e1-543">工作 3-使用新的 ViewModel StoreController 中</span><span class="sxs-lookup"><span data-stu-id="004e1-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="004e1-544">在這個工作中，您將修改**StoreController**的**瀏覽**並**詳細資料**動作方法，以使用新**StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="004e1-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="004e1-545">加入 [Models] 資料夾中的參考**StoreController**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="004e1-546">若要這樣做，請展開**控制器**資料夾中的**方案總管**，然後開啟**StoreController**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="004e1-547">然後新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="004e1-547">Then add the following code:</span></span>

    <span data-ttu-id="004e1-548">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="004e1-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="004e1-549">取代**瀏覽**動作方法，以使用**StoreViewBrowseController**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="004e1-550">您將建立內容類型和兩個新的專輯物件與虛擬資料 （在下一步 的實際操作實驗室中您會使用資料庫中的實際資料）。</span><span class="sxs-lookup"><span data-stu-id="004e1-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="004e1-551">若要這樣做，請取代**瀏覽**為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="004e1-552">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="004e1-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="004e1-553">取代**詳細資料**動作方法，以使用**StoreViewBrowseController**類別。</span><span class="sxs-lookup"><span data-stu-id="004e1-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="004e1-554">您將建立新**專輯**物件傳回給**檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="004e1-555">若要這樣做，請取代**詳細資料**為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="004e1-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="004e1-556">(程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="004e1-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="004e1-557">工作 4-新增瀏覽檢視範本</span><span class="sxs-lookup"><span data-stu-id="004e1-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="004e1-558">在這個工作中，您會將**瀏覽**檢視，以顯示特定內容類型為找到的 Album。</span><span class="sxs-lookup"><span data-stu-id="004e1-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="004e1-559">之前建立新的檢視範本，您應該會建置專案，讓**加入檢視** 對話方塊之 snmp v1 **ViewModel**類別使用。</span><span class="sxs-lookup"><span data-stu-id="004e1-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="004e1-560">建置專案，方法是選取**建置**功能表項目，然後**建置 MvcMusicStore**。</span><span class="sxs-lookup"><span data-stu-id="004e1-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="004e1-561">新增**瀏覽**檢視。</span><span class="sxs-lookup"><span data-stu-id="004e1-561">Add a **Browse** View.</span></span> <span data-ttu-id="004e1-562">若要這樣做，以滑鼠右鍵按一下**瀏覽**動作方法的**StoreController**然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="004e1-563">在 **加入檢視**對話方塊方塊中，確認檢視名稱**瀏覽**。</span><span class="sxs-lookup"><span data-stu-id="004e1-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="004e1-564">請檢查**建立強型別檢視**核取方塊，然後選取**StoreBrowseViewModel**從**模型類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="004e1-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="004e1-565">其他欄位保留其預設值。</span><span class="sxs-lookup"><span data-stu-id="004e1-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="004e1-566">然後按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="004e1-566">Then click **Add**.</span></span>

    <span data-ttu-id="004e1-567">![加入瀏覽檢視](aspnet-mvc-4-fundamentals/_static/image29.png "加入瀏覽檢視")</span><span class="sxs-lookup"><span data-stu-id="004e1-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="004e1-568">*加入瀏覽檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="004e1-569">修改**Browse.cshtml**要顯示的內容類型的詳細資訊，存取**StoreBrowseViewModel**傳遞至檢視範本的物件。</span><span class="sxs-lookup"><span data-stu-id="004e1-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="004e1-570">若要這樣做，請將內容取代為下列: (C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="004e1-571">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="004e1-572">在這個工作中，您將測試所**瀏覽**方法會擷取來自的專輯**瀏覽**方法動作。</span><span class="sxs-lookup"><span data-stu-id="004e1-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="004e1-573">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-574">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="004e1-574">The project starts in the Home page.</span></span> <span data-ttu-id="004e1-575">將 URL 變更為  **/儲存] / [瀏覽？內容類型 = Disco**以確認此動作會傳回兩個相簿。</span><span class="sxs-lookup"><span data-stu-id="004e1-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="004e1-576">![瀏覽市集 Disco 專輯](aspnet-mvc-4-fundamentals/_static/image30.png "瀏覽市集 Disco 相簿")</span><span class="sxs-lookup"><span data-stu-id="004e1-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="004e1-577">*瀏覽市集 Disco 相簿*</span><span class="sxs-lookup"><span data-stu-id="004e1-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="004e1-578">工作 6-顯示資訊的相關特定專輯</span><span class="sxs-lookup"><span data-stu-id="004e1-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="004e1-579">在這個工作中，您將實作**存放區/詳細資料**檢視，以顯示特定專輯的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="004e1-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="004e1-580">在這個實際操作實驗室中，您將會顯示關於專輯的所有項目已經包含在**檢視**範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="004e1-581">是的而不是建立**StoreDetailsViewModel**類別，您將使用目前**StoreBrowseViewModel**將專輯傳遞給它的範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="004e1-582">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="004e1-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="004e1-583">加入新**詳細資料**檢視**StoreController**的**詳細資料**動作方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="004e1-584">若要這樣做，請以滑鼠右鍵按一下**詳細資料**方法中的**StoreController**類別，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="004e1-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="004e1-585">在 [**加入檢視**] 對話方塊中，確認**檢視名稱**是**詳細資料**。</span><span class="sxs-lookup"><span data-stu-id="004e1-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="004e1-586">請檢查**建立強型別檢視**核取方塊，然後選取**專輯**從**模型類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="004e1-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="004e1-587">其他欄位保留其預設值。</span><span class="sxs-lookup"><span data-stu-id="004e1-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="004e1-588">然後按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="004e1-588">Then click **Add**.</span></span> <span data-ttu-id="004e1-589">這會建立並開啟**\Views\Store\Details.cshtml**檔案。</span><span class="sxs-lookup"><span data-stu-id="004e1-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="004e1-590">![加入詳細資料檢視](aspnet-mvc-4-fundamentals/_static/image31.png "加入詳細資料檢視")</span><span class="sxs-lookup"><span data-stu-id="004e1-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="004e1-591">*加入詳細資料檢視*</span><span class="sxs-lookup"><span data-stu-id="004e1-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="004e1-592">修改**Details.cshtml**若要顯示的專輯資訊的檔案存取**專輯**傳遞至檢視範本的物件。</span><span class="sxs-lookup"><span data-stu-id="004e1-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="004e1-593">若要這樣做，請將內容取代為下列: (C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="004e1-594">工作 7-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="004e1-595">在這個工作中，您將測試所**詳細資料**View 擷取來自的專輯資訊**詳細資料動作**方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="004e1-596">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-597">在專案開始**首頁**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="004e1-598">將 URL 變更為 **/Store/Details/5**確認的專輯資訊。</span><span class="sxs-lookup"><span data-stu-id="004e1-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="004e1-599">![瀏覽專輯詳細](aspnet-mvc-4-fundamentals/_static/image32.png "瀏覽專輯詳細資料")</span><span class="sxs-lookup"><span data-stu-id="004e1-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="004e1-600">*瀏覽專輯的詳細資料*</span><span class="sxs-lookup"><span data-stu-id="004e1-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="004e1-601">工作 8-新增頁面之間的連結</span><span class="sxs-lookup"><span data-stu-id="004e1-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="004e1-602">在這個工作中，您將加入在存放區檢視中，將連結放在適當的每個內容類型名稱的連結 **/Store/瀏覽**URL。</span><span class="sxs-lookup"><span data-stu-id="004e1-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="004e1-603">如此一來，當您按一下 內容類型，例如**Disco**，它會瀏覽至**存放區/瀏覽？ 內容類型 = Disco** URL。</span><span class="sxs-lookup"><span data-stu-id="004e1-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="004e1-604">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="004e1-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="004e1-605">更新**Index**新增至連結的頁面**瀏覽**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="004e1-606">若要這樣做，請在**方案總管**展開**檢視**資料夾，則**存放區**資料夾，然後按兩下**Index.cshtml**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="004e1-607">表示內容類型選取 [瀏覽] 檢視中加入的連結。</span><span class="sxs-lookup"><span data-stu-id="004e1-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="004e1-608">若要這樣做，請取代下列反白顯示的程式碼，傳入**&lt;li&gt;** 標記: (C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="004e1-609">另一種方法就直接連結到頁面上，使用程式碼，如下所示：</span><span class="sxs-lookup"><span data-stu-id="004e1-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="004e1-610">&lt;a =&quot;/Store/瀏覽？ 內容類型 =@genreName&quot;&gt;@genreName &lt; /a&gt;</span><span class="sxs-lookup"><span data-stu-id="004e1-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="004e1-611">雖然這種方法的運作方式，它會相依於硬式編碼字串。</span><span class="sxs-lookup"><span data-stu-id="004e1-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="004e1-612">如果您稍後重新命名該控制站，您必須手動變更這項指示。</span><span class="sxs-lookup"><span data-stu-id="004e1-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="004e1-613">更好的替代做法是使用**HTML 協助程式**方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="004e1-614">ASP.NET MVC 包含 HTML 協助程式方法，這是適用於這類工作的方法。</span><span class="sxs-lookup"><span data-stu-id="004e1-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="004e1-615">**Html.ActionLink()** 協助程式方法可讓您輕鬆地建置 HTML **&lt;&gt;** 連結，並確定具有正確的 URL 路徑 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="004e1-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="004e1-616">Htlm.ActionLink 有數個多載。</span><span class="sxs-lookup"><span data-stu-id="004e1-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="004e1-617">在本練習會使用另一個會採用三個參數：</span><span class="sxs-lookup"><span data-stu-id="004e1-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="004e1-618">連結文字，會顯示的內容類型名稱</span><span class="sxs-lookup"><span data-stu-id="004e1-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="004e1-619">控制器動作名稱 (**瀏覽**)</span><span class="sxs-lookup"><span data-stu-id="004e1-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="004e1-620">路由參數值，指定兩個名稱 (**Genre**) 和值 (**內容類型名稱**)</span><span class="sxs-lookup"><span data-stu-id="004e1-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="004e1-621">工作 9-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="004e1-622">在這個工作中，您將測試每個內容類型會顯示適當的連結 **/Store/瀏覽**URL。</span><span class="sxs-lookup"><span data-stu-id="004e1-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="004e1-623">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-624">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="004e1-624">The project starts in the Home page.</span></span> <span data-ttu-id="004e1-625">將 URL 變更為 **/儲存**若要確認每個內容類型連結至適當**存放區/瀏覽**URL。</span><span class="sxs-lookup"><span data-stu-id="004e1-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="004e1-626">![瀏覽連結來瀏覽 頁面的內容類型](aspnet-mvc-4-fundamentals/_static/image33.png "連結來瀏覽] 頁面的 [瀏覽內容類型")</span><span class="sxs-lookup"><span data-stu-id="004e1-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="004e1-627">*瀏覽的內容類型瀏覽 頁面的連結*</span><span class="sxs-lookup"><span data-stu-id="004e1-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="004e1-628">工作 10-使用動態 ViewModel 集合來傳遞值</span><span class="sxs-lookup"><span data-stu-id="004e1-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="004e1-629">在這個工作中，您將了解簡單且功能強大的方法，將控制器和檢視之間傳遞值，而不進行任何變更，在模型中。</span><span class="sxs-lookup"><span data-stu-id="004e1-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="004e1-630">ASP.NET MVC 4 提供集合&quot;ViewModel&quot;，這可以指派給任何動態的值和控制器和檢視以及內存取。</span><span class="sxs-lookup"><span data-stu-id="004e1-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="004e1-631">您現在會使用 ViewBag 動態集合，將一份&quot;**加星號內容類型**&quot;從控制器到檢視。</span><span class="sxs-lookup"><span data-stu-id="004e1-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="004e1-632">將存取存放區索引檢視，以**ViewModel**並顯示資訊。</span><span class="sxs-lookup"><span data-stu-id="004e1-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="004e1-633">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="004e1-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="004e1-634">開啟**StoreController.cs** ，並修改**Index**方法用來建立一份起子頭插入 ViewModel 集合的內容類型：</span><span class="sxs-lookup"><span data-stu-id="004e1-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="004e1-635">您也可以使用語法**ViewBag [&quot;Starred&quot;]** 來存取屬性。</span><span class="sxs-lookup"><span data-stu-id="004e1-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="004e1-636">星狀圖示**&quot;starred.png&quot;** 納入**Source\Assets\Images**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="004e1-637">若要將它新增至應用程式中，拖曳其內容從**Windows 檔案總管**到視窗**方案總管 中**在 Visual Web Developer Express，如下所示：</span><span class="sxs-lookup"><span data-stu-id="004e1-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="004e1-638">![加入至方案的星型映像](aspnet-mvc-4-fundamentals/_static/image34.png "星星的影像加入至方案")</span><span class="sxs-lookup"><span data-stu-id="004e1-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="004e1-639">*星型映像新增到方案*</span><span class="sxs-lookup"><span data-stu-id="004e1-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="004e1-640">開啟檢視**Store/Index.cshtml**和修改內容。</span><span class="sxs-lookup"><span data-stu-id="004e1-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="004e1-641">您將讀取&quot;加星號&quot;中的屬性**ViewBag**集合，並詢問是否在清單中目前的內容類型名稱。</span><span class="sxs-lookup"><span data-stu-id="004e1-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="004e1-642">在此情況下，您會顯示由右至內容類型連結以星號圖示。</span><span class="sxs-lookup"><span data-stu-id="004e1-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="004e1-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="004e1-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="004e1-644">工作 11-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="004e1-645">在這個工作中，您將測試的星號的內容類型，顯示星星的圖示。</span><span class="sxs-lookup"><span data-stu-id="004e1-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="004e1-646">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="004e1-647">在專案開始**首頁**頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="004e1-648">將 URL 變更為 **/儲存**以確認每個精選的內容類型有 respecting 標籤：</span><span class="sxs-lookup"><span data-stu-id="004e1-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="004e1-649">![瀏覽內容類型，具有星號的項目](aspnet-mvc-4-fundamentals/_static/image35.png "瀏覽的內容類型與星號的項目")</span><span class="sxs-lookup"><span data-stu-id="004e1-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="004e1-650">*瀏覽的內容類型與星號的項目*</span><span class="sxs-lookup"><span data-stu-id="004e1-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="004e1-651">練習 7: 瀏覽 ASP.NET MVC 4 的新範本</span><span class="sxs-lookup"><span data-stu-id="004e1-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="004e1-652">在此練習中，您會瀏覽的增強功能，在 ASP.NET MVC 4 專案範本中，初步了解新範本的最相關的功能。</span><span class="sxs-lookup"><span data-stu-id="004e1-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="004e1-653">工作 1： 瀏覽 ASP.NET MVC 4 的網際網路應用程式範本</span><span class="sxs-lookup"><span data-stu-id="004e1-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="004e1-654">如果尚未開啟，啟動**VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="004e1-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="004e1-655">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="004e1-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="004e1-656">在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="004e1-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="004e1-657">**名稱**專案*MusicStore* ，並更新**方案名稱**來*開始*，然後選取位置 （或保留預設值），按一下  **確定**.</span><span class="sxs-lookup"><span data-stu-id="004e1-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="004e1-658">![建立新的 ASP.NET MVC 4 專案](aspnet-mvc-4-fundamentals/_static/image36.png "建立新的 ASP.NET MVC 4 專案")</span><span class="sxs-lookup"><span data-stu-id="004e1-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="004e1-659">*建立新的 ASP.NET MVC 4 專案*</span><span class="sxs-lookup"><span data-stu-id="004e1-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="004e1-660">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="004e1-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="004e1-661">請注意，您可以選取 ASPX 或 Razor 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="004e1-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="004e1-662">![建立新 ASP.NET MVC 4 網際網路應用程式](aspnet-mvc-4-fundamentals/_static/image37.png "建立新 ASP.NET MVC 4 網際網路應用程式")</span><span class="sxs-lookup"><span data-stu-id="004e1-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="004e1-663">*建立新 ASP.NET MVC 4 網際網路應用程式*</span><span class="sxs-lookup"><span data-stu-id="004e1-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-664">ASP.NET MVC 3 中已引進 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="004e1-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="004e1-665">其目標是要減少字元和啟用快速和流暢的程式碼撰寫工作流程在檔案中，所需按鍵的數目。</span><span class="sxs-lookup"><span data-stu-id="004e1-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="004e1-666">Razor 會利用現有 C# /VB （或其他） 的語言能力，並提供可讓一個很棒的 HTML 建構工作流程的範本標記語法。</span><span class="sxs-lookup"><span data-stu-id="004e1-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="004e1-667">按下**F5**執行方案，並查看更新的範本。</span><span class="sxs-lookup"><span data-stu-id="004e1-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="004e1-668">您可以查看下列功能：</span><span class="sxs-lookup"><span data-stu-id="004e1-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="004e1-669">**現代化樣式範本**</span><span class="sxs-lookup"><span data-stu-id="004e1-669">**Modern-style templates**</span></span>

        <span data-ttu-id="004e1-670">範本已經更新，提供更多的現代化外觀的樣式。</span><span class="sxs-lookup"><span data-stu-id="004e1-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="004e1-671">![重新設定樣式的 ASP.NET MVC 4 範本](aspnet-mvc-4-fundamentals/_static/image38.png "抵擋範本的 ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="004e1-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="004e1-672">*重新設定樣式的 ASP.NET MVC 4 範本*</span><span class="sxs-lookup"><span data-stu-id="004e1-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="004e1-673">**自適性轉譯**</span><span class="sxs-lookup"><span data-stu-id="004e1-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="004e1-674">請參閱調整瀏覽器視窗的大小，並注意如何頁面配置動態調整為新的視窗大小。</span><span class="sxs-lookup"><span data-stu-id="004e1-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="004e1-675">這些範本會使用適應性呈現技巧，來正確轉譯，而不需要任何自訂的桌上型電腦和行動裝置平台。</span><span class="sxs-lookup"><span data-stu-id="004e1-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="004e1-676">![ASP.NET MVC 4 專案範本，以不同的瀏覽器尺寸](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 專案範本，在不同的瀏覽器大小")</span><span class="sxs-lookup"><span data-stu-id="004e1-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="004e1-677">*ASP.NET MVC 4 專案範本，在不同的瀏覽器大小*</span><span class="sxs-lookup"><span data-stu-id="004e1-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="004e1-678">關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="004e1-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="004e1-679">現在您已經能夠探索解決方案，並查看一些推出由 ASP.NET MVC 4 專案範本的新功能。</span><span class="sxs-lookup"><span data-stu-id="004e1-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="004e1-680">![ASP.NET MVC4-網際網路-應用程式-專案-樣板](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 的網際網路應用程式專案範本")</span><span class="sxs-lookup"><span data-stu-id="004e1-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="004e1-681">*ASP.NET MVC 4 的網際網路應用程式專案範本*</span><span class="sxs-lookup"><span data-stu-id="004e1-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="004e1-682">**HTML5 標記**</span><span class="sxs-lookup"><span data-stu-id="004e1-682">**HTML5 markup**</span></span>

       <span data-ttu-id="004e1-683">瀏覽以找出新的佈景主題標記，例如開啟的範本檢視**About.cshtml**檢視中的**首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="004e1-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="004e1-684">![新的範本，使用 Razor 和 HTML5 的標記](aspnet-mvc-4-fundamentals/_static/image41.png "新的範本，使用 Razor 和 HTML5 的標記")</span><span class="sxs-lookup"><span data-stu-id="004e1-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="004e1-685">*新的範本，使用 Razor 和 HTML5 的標記*</span><span class="sxs-lookup"><span data-stu-id="004e1-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="004e1-686">**包含的 JavaScript 程式庫**</span><span class="sxs-lookup"><span data-stu-id="004e1-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="004e1-687">**jQuery**: jQuery 簡化 HTML 文件周遊、 事件處理、 加上動畫，以及 Ajax 互動。</span><span class="sxs-lookup"><span data-stu-id="004e1-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="004e1-688">**jQuery UI**： 此程式庫提供低階互動和進階效果的動畫，以及主題化 widget，jQuery JavaScript 程式庫為基礎的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="004e1-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="004e1-689">您可以了解 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。</span><span class="sxs-lookup"><span data-stu-id="004e1-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="004e1-690">**KnockoutJS**： 現在包含 ASP.NET MVC 4 預設範本**KnockoutJS**，可讓您建立豐富且回應迅速的 web 應用程式使用 JavaScript 和 HTML 的 JavaScript MVVM 架構。</span><span class="sxs-lookup"><span data-stu-id="004e1-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="004e1-691">例如在 ASP.NET MVC 3 中，jQuery 和 jQuery UI 程式庫也包含 ASP.NET MVC 4 中。</span><span class="sxs-lookup"><span data-stu-id="004e1-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="004e1-692">您可以取得 KnockOutJS 程式庫，在下列連結的詳細資訊： [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/)。</span><span class="sxs-lookup"><span data-stu-id="004e1-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="004e1-693">**Modernizr**： 此程式庫會自動執行，讓您的網站與較舊的瀏覽器相容，使用 HTML5 和 css3 等技術時。</span><span class="sxs-lookup"><span data-stu-id="004e1-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="004e1-694">您可以取得此連結中的 Modernizr 程式庫的詳細資訊： [ http://www.modernizr.com/ ](http://www.modernizr.com/)。</span><span class="sxs-lookup"><span data-stu-id="004e1-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="004e1-695">**在方案中包含的 SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="004e1-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="004e1-696">SimpleMembership 已設計為先前的 ASP.NET 角色和成員資格提供者系統取代。</span><span class="sxs-lookup"><span data-stu-id="004e1-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="004e1-697">它有許多新功能，輕鬆安全的網頁開發人員以更有彈性的方式。</span><span class="sxs-lookup"><span data-stu-id="004e1-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="004e1-698">網際網路範本已經有設定整合 SimpleMembership 幾件事，例如 AccountController 已準備好要使用 OAuthWebSecurity （適用於 OAuth 的帳戶註冊、 登入、 管理等） 和 Web 的安全性。</span><span class="sxs-lookup"><span data-stu-id="004e1-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="004e1-699">![包含在解決方案中 SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership 納入解決方案")</span><span class="sxs-lookup"><span data-stu-id="004e1-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="004e1-700">*包含在解決方案中 SimpleMembership*</span><span class="sxs-lookup"><span data-stu-id="004e1-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="004e1-701">進一步了解[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN 中。</span><span class="sxs-lookup"><span data-stu-id="004e1-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="004e1-702">此外，您可以在其中部署此應用程式以 Windows Azure 網站的下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。</span><span class="sxs-lookup"><span data-stu-id="004e1-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="004e1-703">總結</span><span class="sxs-lookup"><span data-stu-id="004e1-703">Summary</span></span>

<span data-ttu-id="004e1-704">藉由完成這個實際操作實驗室中，您已了解 ASP.NET MVC 的基本概念：</span><span class="sxs-lookup"><span data-stu-id="004e1-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="004e1-705">MVC 應用程式，以及它們如何互動的核心項目</span><span class="sxs-lookup"><span data-stu-id="004e1-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="004e1-706">如何建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="004e1-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="004e1-707">如何新增和設定控制站來處理參數傳遞的 URL 和查詢字串</span><span class="sxs-lookup"><span data-stu-id="004e1-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="004e1-708">如何新增版面配置主版頁面，設定一般的 HTML 內容，以增強外觀與風格及檢視範本，以顯示 HTML 內容的樣式表的範本</span><span class="sxs-lookup"><span data-stu-id="004e1-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="004e1-709">如何將屬性傳遞至檢視範本中使用 ViewModel 模式，來顯示動態資訊</span><span class="sxs-lookup"><span data-stu-id="004e1-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="004e1-710">如何使用傳遞至控制器檢視範本參數</span><span class="sxs-lookup"><span data-stu-id="004e1-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="004e1-711">如何將連結新增至 ASP.NET MVC 應用程式內的頁面</span><span class="sxs-lookup"><span data-stu-id="004e1-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="004e1-712">如何加入和檢視中使用動態屬性</span><span class="sxs-lookup"><span data-stu-id="004e1-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="004e1-713">ASP.NET MVC 4 專案範本中的增強功能</span><span class="sxs-lookup"><span data-stu-id="004e1-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="004e1-714">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="004e1-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="004e1-715">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="004e1-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="004e1-716">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="004e1-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="004e1-717">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="004e1-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="004e1-718">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="004e1-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="004e1-719">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="004e1-719">Click on **Install Now**.</span></span> <span data-ttu-id="004e1-720">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="004e1-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="004e1-721">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="004e1-722">![安裝 Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="004e1-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="004e1-723">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="004e1-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="004e1-724">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="004e1-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="004e1-726">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="004e1-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="004e1-727">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="004e1-727">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="004e1-729">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="004e1-729">*Installation progress*</span></span>
6. <span data-ttu-id="004e1-730">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="004e1-730">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="004e1-732">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="004e1-732">*Installation completed*</span></span>
7. <span data-ttu-id="004e1-733">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="004e1-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="004e1-734">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="004e1-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="004e1-736">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="004e1-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="004e1-737">附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="004e1-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="004e1-738">本附錄將說明如何從 Windows Azure 管理入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="004e1-739">工作 1-建立新的網站，從 Windows Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="004e1-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="004e1-740">移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="004e1-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-741">使用 Windows Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。</span><span class="sxs-lookup"><span data-stu-id="004e1-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="004e1-742">您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="004e1-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="004e1-743">![登入 Windows Azure 入口網站](aspnet-mvc-4-fundamentals/_static/image48.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="004e1-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="004e1-744">*登入 Windows Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="004e1-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="004e1-745">按一下 **新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="004e1-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="004e1-746">![建立新的 Web 站台](aspnet-mvc-4-fundamentals/_static/image49.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="004e1-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="004e1-747">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="004e1-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="004e1-748">按一下 **計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="004e1-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="004e1-749">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="004e1-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="004e1-750">新的網站提供可用的 URL，然後按**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="004e1-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-751">Windows Azure 網站時，您可以控制和管理雲端中執行的 web 應用程式的主應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="004e1-752">[快速建立] 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="004e1-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="004e1-753">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="004e1-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="004e1-754">![建立新的網站上，使用 快速建立](aspnet-mvc-4-fundamentals/_static/image50.png "建立新的網站上，使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="004e1-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="004e1-755">*建立新的網站上，使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="004e1-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="004e1-756">等到新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="004e1-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="004e1-757">建立網站後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="004e1-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="004e1-758">檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="004e1-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="004e1-759">![瀏覽至新的 web 站台](aspnet-mvc-4-fundamentals/_static/image51.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="004e1-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="004e1-760">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="004e1-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="004e1-761">![執行的 web 站台](aspnet-mvc-4-fundamentals/_static/image52.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="004e1-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="004e1-762">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="004e1-762">*Web site running*</span></span>
6. <span data-ttu-id="004e1-763">返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="004e1-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="004e1-764">![開啟 網站管理頁面](aspnet-mvc-4-fundamentals/_static/image53.png "開啟的網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="004e1-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="004e1-765">*開啟 網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="004e1-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="004e1-766">在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="004e1-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-767">*發行設定檔*包含所有發行至 Windows Azure 網站的每個已啟用的發行方法的 web 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="004e1-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="004e1-768">發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。</span><span class="sxs-lookup"><span data-stu-id="004e1-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="004e1-769">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Windows Azure 網站的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="004e1-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="004e1-770">![正在下載網站發行設定檔](aspnet-mvc-4-fundamentals/_static/image54.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="004e1-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="004e1-771">*正在下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="004e1-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="004e1-772">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="004e1-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="004e1-773">進一步在這個練習中，您會看到如何從 Visual Studio 的 web 應用程式至 Windows Azure 網站發行使用此檔案。</span><span class="sxs-lookup"><span data-stu-id="004e1-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="004e1-774">![儲存發行設定檔](aspnet-mvc-4-fundamentals/_static/image55.png "儲存發佈設定檔")</span><span class="sxs-lookup"><span data-stu-id="004e1-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="004e1-775">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="004e1-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="004e1-776">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="004e1-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="004e1-777">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="004e1-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="004e1-778">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="004e1-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="004e1-779">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="004e1-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="004e1-780">您可以從您的訂用帳戶，在 Windows Azure Management 入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器儀表板**。</span><span class="sxs-lookup"><span data-stu-id="004e1-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="004e1-781">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="004e1-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="004e1-782">請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="004e1-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="004e1-783">並未建立資料庫，因為它會建立在稍後的階段。</span><span class="sxs-lookup"><span data-stu-id="004e1-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="004e1-784">![SQL Database 伺服器儀表板](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="004e1-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="004e1-785">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="004e1-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="004e1-786">下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="004e1-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="004e1-787">若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="004e1-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![新增用戶端 IP 位址](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="004e1-789">*新增用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="004e1-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="004e1-790">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="004e1-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="004e1-792">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="004e1-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="004e1-793">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="004e1-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="004e1-794">返回至 ASP.NET MVC 4 的方案。</span><span class="sxs-lookup"><span data-stu-id="004e1-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="004e1-795">在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="004e1-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="004e1-796">![發行應用程式](aspnet-mvc-4-fundamentals/_static/image60.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="004e1-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="004e1-797">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="004e1-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="004e1-798">匯入發行設定檔儲存在第一項工作。</span><span class="sxs-lookup"><span data-stu-id="004e1-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="004e1-799">![匯入發行設定檔](aspnet-mvc-4-fundamentals/_static/image61.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="004e1-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="004e1-800">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="004e1-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="004e1-801">按一下 **驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="004e1-801">Click **Validate Connection**.</span></span> <span data-ttu-id="004e1-802">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="004e1-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="004e1-803">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="004e1-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="004e1-804">![驗證連接](aspnet-mvc-4-fundamentals/_static/image62.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="004e1-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="004e1-805">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="004e1-805">*Validating connection*</span></span>
4. <span data-ttu-id="004e1-806">在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="004e1-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="004e1-807">![Web 部署組態](aspnet-mvc-4-fundamentals/_static/image63.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="004e1-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="004e1-808">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="004e1-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="004e1-809">設定資料庫連線，如下所示：</span><span class="sxs-lookup"><span data-stu-id="004e1-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="004e1-810">在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。</span><span class="sxs-lookup"><span data-stu-id="004e1-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="004e1-811">在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="004e1-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="004e1-812">在 **密碼**輸入您的伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="004e1-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="004e1-813">輸入新的資料庫名稱，例如： *MVC4SampleDB*。</span><span class="sxs-lookup"><span data-stu-id="004e1-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="004e1-814">![設定目的地連接字串](aspnet-mvc-4-fundamentals/_static/image64.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="004e1-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="004e1-815">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="004e1-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="004e1-816">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="004e1-816">Then click **OK**.</span></span> <span data-ttu-id="004e1-817">當系統提示您建立資料庫時，按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="004e1-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="004e1-818">![建立資料庫](aspnet-mvc-4-fundamentals/_static/image65.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="004e1-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="004e1-819">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="004e1-819">*Creating the database*</span></span>
7. <span data-ttu-id="004e1-820">您將用來連接至 Windows Azure 中的 SQL 資料庫的連接字串會顯示在文字方塊中的預設連線。</span><span class="sxs-lookup"><span data-stu-id="004e1-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="004e1-821">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="004e1-821">Then click **Next**.</span></span>

    <span data-ttu-id="004e1-822">![連接字串指向 SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "連接字串指向 SQL Database")</span><span class="sxs-lookup"><span data-stu-id="004e1-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="004e1-823">*連接字串指向 SQL Database*</span><span class="sxs-lookup"><span data-stu-id="004e1-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="004e1-824">在  **Preview**頁面上，按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="004e1-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="004e1-825">![Web 應用程式發行](aspnet-mvc-4-fundamentals/_static/image67.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="004e1-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="004e1-826">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="004e1-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="004e1-827">當發行程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="004e1-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="004e1-828">![應用程式發行至 Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "應用程式發行至 Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="004e1-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="004e1-829">*應用程式發佈至 Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="004e1-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="004e1-830">附錄 c： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="004e1-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="004e1-831">使用程式碼片段，您會有您需要在隨手可得的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="004e1-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="004e1-832">實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="004e1-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="004e1-833">![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-fundamentals/_static/image69.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="004e1-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="004e1-834">*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="004e1-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="004e1-835">***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="004e1-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="004e1-836">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="004e1-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="004e1-837">開始輸入程式碼片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="004e1-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="004e1-838">觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="004e1-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="004e1-839">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="004e1-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="004e1-840">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="004e1-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="004e1-841">![開始鍵入程式碼片段名稱](aspnet-mvc-4-fundamentals/_static/image70.png "開始輸入程式碼片段名稱")</span><span class="sxs-lookup"><span data-stu-id="004e1-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="004e1-842">*開始鍵入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="004e1-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="004e1-843">![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-fundamentals/_static/image71.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="004e1-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="004e1-844">*按 Tab 鍵以選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="004e1-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="004e1-845">![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-fundamentals/_static/image72.png "再次按 Tab 鍵和程式碼片段會展開")</span><span class="sxs-lookup"><span data-stu-id="004e1-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="004e1-846">*再次按 Tab 鍵和程式碼片段會展開*</span><span class="sxs-lookup"><span data-stu-id="004e1-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="004e1-847">***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="004e1-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="004e1-848">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="004e1-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="004e1-849">選取 **插入程式碼片段**後面**My Code Snippets**。</span><span class="sxs-lookup"><span data-stu-id="004e1-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="004e1-850">選擇相關的程式片段，從清單中，對它按一下。</span><span class="sxs-lookup"><span data-stu-id="004e1-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="004e1-851">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-fundamentals/_static/image73.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="004e1-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="004e1-852">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="004e1-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="004e1-853">![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-fundamentals/_static/image74.png "對它按一下挑選清單中，相關程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="004e1-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="004e1-854">*對它按一下挑選清單中，相關程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="004e1-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
