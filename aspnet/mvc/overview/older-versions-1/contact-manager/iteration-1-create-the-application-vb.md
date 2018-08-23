---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: '反覆項目 #1 – 建立應用程式 (VB) |Microsoft Docs'
author: microsoft
description: 在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和 D...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 9369f843719d7198716ff83c5bbd5d3995f70973
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825570"
---
<a name="iteration-1--create-the-application-vb"></a><span data-ttu-id="06413-104">反覆項目 #1 – 建立應用程式 (VB)</span><span class="sxs-lookup"><span data-stu-id="06413-104">Iteration #1 – Create the Application (VB)</span></span>
====================
<span data-ttu-id="06413-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="06413-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="06413-106">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="06413-106">Download Code</span></span>](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> <span data-ttu-id="06413-107">在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="06413-107">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="06413-108">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="06413-108">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="06413-109">建立連絡人管理 ASP.NET MVC 應用程式 (VB)</span><span class="sxs-lookup"><span data-stu-id="06413-109">Building a Contact Management ASP.NET MVC Application (VB)</span></span>

<span data-ttu-id="06413-110">在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="06413-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="06413-111">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="06413-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="06413-112">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="06413-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="06413-113">每次反覆運算時，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="06413-114">此多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="06413-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="06413-115">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="06413-116">在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="06413-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="06413-117">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="06413-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="06413-118">反覆項目 #2-讓應用程式看起來不錯。</span><span class="sxs-lookup"><span data-stu-id="06413-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="06413-119">這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="06413-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="06413-120">反覆項目 #3-新增表單驗證。</span><span class="sxs-lookup"><span data-stu-id="06413-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="06413-121">在第三個反覆項目，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="06413-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="06413-122">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="06413-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="06413-123">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="06413-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="06413-124">反覆項目 #4-進行鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="06413-125">在這個第四個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-125">In this fourth iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="06413-126">比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="06413-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="06413-127">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="06413-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="06413-128">在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。</span><span class="sxs-lookup"><span data-stu-id="06413-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="06413-129">我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。</span><span class="sxs-lookup"><span data-stu-id="06413-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="06413-130">反覆項目 #6-使用測試導向開發。</span><span class="sxs-lookup"><span data-stu-id="06413-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="06413-131">在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="06413-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="06413-132">在這個反覆項目，我們會新增連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="06413-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="06413-133">反覆項目 #7-新增 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="06413-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="06413-134">在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="06413-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="06413-135">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="06413-135">This Iteration</span></span>

<span data-ttu-id="06413-136">在這個第一個反覆項目中，我們會建置基本的應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-136">In this first iteration, we build the basic application.</span></span> <span data-ttu-id="06413-137">目標是要以最快、 最簡單的方式建置連絡管理員。</span><span class="sxs-lookup"><span data-stu-id="06413-137">The goal is to build the Contact Manager in the fastest and simplest way possible.</span></span> <span data-ttu-id="06413-138">在後期反覆項目，我們可以改善應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="06413-138">In later iterations, we improve the design of the application.</span></span>

<span data-ttu-id="06413-139">請連絡系統管理員應用程式是基本的資料庫導向應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-139">The Contact Manager application is a basic database-driven application.</span></span> <span data-ttu-id="06413-140">您可以使用應用程式建立新的連絡人、 編輯現有的連絡人，以及刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-140">You can use the application to create new contacts, edit existing contacts, and delete contacts.</span></span>

<span data-ttu-id="06413-141">在這個反覆項目，我們會完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="06413-141">In this iteration, we complete the following steps:</span></span>

1. <span data-ttu-id="06413-142">ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="06413-142">ASP.NET MVC application</span></span>
2. <span data-ttu-id="06413-143">建立資料庫來儲存我們的連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-143">Create a database to store our contacts</span></span>
3. <span data-ttu-id="06413-144">產生 Microsoft Entity Framework 與資料庫的模型類別</span><span class="sxs-lookup"><span data-stu-id="06413-144">Generate a model class for our database with the Microsoft Entity Framework</span></span>
4. <span data-ttu-id="06413-145">建立控制器動作和檢視，可以讓我們來列出所有資料庫中的連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-145">Create a controller action and view that enables us to list all of the contacts in the database</span></span>
5. <span data-ttu-id="06413-146">建立控制器的動作和檢視，可讓我們在資料庫中建立新的連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-146">Create controller actions and a view that enables us to create a new contact in the database</span></span>
6. <span data-ttu-id="06413-147">建立控制器的動作和檢視，可讓我們編輯資料庫中現有的連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-147">Create controller actions and a view that enables us to edit an existing contact in the database</span></span>
7. <span data-ttu-id="06413-148">建立控制器的動作和檢視，可讓我們刪除資料庫中現有的連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-148">Create controller actions and a view that enables us to delete an existing contact in the database</span></span>

## <a name="software-prerequisites"></a><span data-ttu-id="06413-149">軟體必要條件</span><span class="sxs-lookup"><span data-stu-id="06413-149">Software Prerequisites</span></span>

<span data-ttu-id="06413-150">在 ASP.NET MVC 應用程式中，您必須有 Visual Studio 2008 或 Visual Web Developer 2008 （Visual Web Developer 是免費版不包含的所有進階功能的 Visual Studio 的 Visual Studio） 在電腦上安裝。</span><span class="sxs-lookup"><span data-stu-id="06413-150">In ASP.NET MVC applications, you must have either Visual Studio 2008 or Visual Web Developer 2008 installed on your computer (Visual Web Developer is a free version of Visual Studio that does not include all of the advanced features of Visual Studio).</span></span> <span data-ttu-id="06413-151">您可以從下列網址下載是 Visual Studio 2008 試用版或 Visual Web Developer:</span><span class="sxs-lookup"><span data-stu-id="06413-151">You can download either the trial version of Visual Studio 2008 or Visual Web Developer from the following address:</span></span>

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> <span data-ttu-id="06413-152">ASP.NET MVC 應用程式與 Visual Web Developer 中，您必須使用 Visual Web Developer 安裝 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="06413-152">For ASP.NET MVC applications with Visual Web Developer, you must have Visual Web Developer Service Pack 1 installed.</span></span> <span data-ttu-id="06413-153">不含 Service Pack 1，您無法建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="06413-153">Without Service Pack 1, you cannot create Web Application Projects.</span></span>


<span data-ttu-id="06413-154">ASP.NET MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="06413-154">ASP.NET MVC framework.</span></span> <span data-ttu-id="06413-155">您可以從下列網址下載 ASP.NET MVC 架構：</span><span class="sxs-lookup"><span data-stu-id="06413-155">You can download the ASP.NET MVC framework from the following address:</span></span>

[https://www.asp.net/mvc](../../../index.md)

<span data-ttu-id="06413-156">在本教學課程中，我們會使用 Microsoft Entity Framework 存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-156">In this tutorial, we use the Microsoft Entity Framework to access a database.</span></span> <span data-ttu-id="06413-157">Entity Framework 是隨附於.NET Framework 3.5 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="06413-157">The Entity Framework is included with .NET Framework 3.5 Service Pack 1.</span></span> <span data-ttu-id="06413-158">您可以從下列位置下載這個 service pack:</span><span class="sxs-lookup"><span data-stu-id="06413-158">You can download this service pack from the following location:</span></span>

[<span data-ttu-id="06413-159">https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&ampdisplaylang = en</span><span class="sxs-lookup"><span data-stu-id="06413-159">https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en</span></span>](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

<span data-ttu-id="06413-160">執行每個這些下載一個接著一個替代方案，您可以利用 Web Platform Installer (Web PI)。</span><span class="sxs-lookup"><span data-stu-id="06413-160">As an alternative to performing each of these downloads one by one, you can take advantage of the Web Platform Installer (Web PI).</span></span> <span data-ttu-id="06413-161">您可以從下列網址下載 Web PI:</span><span class="sxs-lookup"><span data-stu-id="06413-161">You can download the Web PI from the following address:</span></span>

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a><span data-ttu-id="06413-162">ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="06413-162">ASP.NET MVC Project</span></span>

<span data-ttu-id="06413-163">ASP.NET MVC Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="06413-163">ASP.NET MVC Web Application Project.</span></span> <span data-ttu-id="06413-164">啟動 Visual Studio，並選取功能表選項**檔案]、 [新增專案**。</span><span class="sxs-lookup"><span data-stu-id="06413-164">Launch Visual Studio and select the menu option **File, New Project**.</span></span> <span data-ttu-id="06413-165">**新的專案**對話方塊隨即出現 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="06413-165">The **New Project** dialog appears (see Figure 1).</span></span> <span data-ttu-id="06413-166">選取  **Web**專案類型並**ASP.NET MVC Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="06413-166">Select the **Web** project type and the **ASP.NET MVC Web Application** template.</span></span> <span data-ttu-id="06413-167">您的新專案命名*ContactManager* ，按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="06413-167">Name your new project *ContactManager* and click the OK button.</span></span>


<span data-ttu-id="06413-168">請確定您已從頂端的下拉式清單中選取的.NET Framework 3.5 角**新的專案**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="06413-168">Make sure that you have .NET Framework 3.5 selected from the dropdown list at the top right of the **New Project** dialog.</span></span> <span data-ttu-id="06413-169">否則，贏得 t 的 ASP.NET MVC Web 應用程式範本會出現。</span><span class="sxs-lookup"><span data-stu-id="06413-169">Otherwise, the ASP.NET MVC Web Application template won t appear.</span></span>


<span data-ttu-id="06413-170">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06413-170">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)</span></span>

<span data-ttu-id="06413-171">**[圖 01**: 新增專案] 對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="06413-171">**Figure 01**: The New Project dialog([Click to view full-size image](iteration-1-create-the-application-vb/_static/image2.png))</span></span>


<span data-ttu-id="06413-172">ASP.NET MVC 應用程式中，**建立單元測試專案**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="06413-172">ASP.NET MVC application, the **Create Unit Test Project** dialog appears.</span></span> <span data-ttu-id="06413-173">您可以使用此對話方塊指出您要建立將單元測試專案新增至您的解決方案，當您建立 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-173">You can use this dialog to indicate that you want to create and add a unit test project to your solution when you create your ASP.NET MVC application.</span></span> <span data-ttu-id="06413-174">雖然我們已贏得 t 建置此反覆項目中的單元測試，您應該選取選項**可以，請建立單元測試專案**由於我們想在稍後的反覆項目中加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="06413-174">Although we won t be building unit tests in this iteration, you should select the option **Yes, create a unit test project** because we plan to add unit tests in a later iteration.</span></span> <span data-ttu-id="06413-175">新增測試專案，當您第一次建立新的 ASP.NET MVC 專案時就能輕鬆與建立 ASP.NET MVC 專案之後，加入測試專案。</span><span class="sxs-lookup"><span data-stu-id="06413-175">Adding a Test project when you first create a new ASP.NET MVC project is much easier than adding a Test project after the ASP.NET MVC project has been created.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="06413-176">Visual Web Developer 不支援測試專案，因為您不會收到建立單元測試專案 對話方塊時使用 Visual Web Developer。</span><span class="sxs-lookup"><span data-stu-id="06413-176">Because Visual Web Developer does not support Test projects, you do not get the Create Unit Test Project dialog when using Visual Web Developer.</span></span>


<span data-ttu-id="06413-177">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="06413-177">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)</span></span>

<span data-ttu-id="06413-178">**[圖 02**: 建立單元測試專案] 對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="06413-178">**Figure 02**: The Create Unit Test Project dialog ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image4.png))</span></span>


<span data-ttu-id="06413-179">ASP.NET MVC 應用程式會出現在 Visual Studio 方案總管 視窗中 （請參閱 圖 3）。</span><span class="sxs-lookup"><span data-stu-id="06413-179">ASP.NET MVC application appears in the Visual Studio Solution Explorer window (see Figure 3).</span></span> <span data-ttu-id="06413-180">如果您不要看到 方案總管 視窗，然後您可以選取功能表選項來開啟此視窗**檢視中，方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="06413-180">If you don t see the Solution Explorer window then you can open this window by selecting the menu option **View, Solution Explorer**.</span></span> <span data-ttu-id="06413-181">請注意，此方案包含兩個專案： 在 ASP.NET MVC 專案和測試專案。</span><span class="sxs-lookup"><span data-stu-id="06413-181">Notice that the solution contains two projects: the ASP.NET MVC project and the Test project.</span></span> <span data-ttu-id="06413-182">ASP.NET MVC 專案的名稱為 ContactManager 和測試專案名為 ContactManager.Tests。</span><span class="sxs-lookup"><span data-stu-id="06413-182">The ASP.NET MVC project is named ContactManager and the Test project is named ContactManager.Tests.</span></span>


<span data-ttu-id="06413-183">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="06413-183">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)</span></span>

<span data-ttu-id="06413-184">**圖 03**: [方案總管] 視窗 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="06413-184">**Figure 03**: The Solution Explorer window ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image6.png))</span></span>


## <a name="deleting-the-project-sample-files"></a><span data-ttu-id="06413-185">刪除專案範例檔案</span><span class="sxs-lookup"><span data-stu-id="06413-185">Deleting the Project Sample Files</span></span>

<span data-ttu-id="06413-186">ASP.NET MVC 專案範本包含控制器和檢視的範例檔案。</span><span class="sxs-lookup"><span data-stu-id="06413-186">The ASP.NET MVC project template includes sample files for controllers and views.</span></span> <span data-ttu-id="06413-187">然後再建立新的 ASP.NET MVC 應用程式，您應該刪除這些檔案。</span><span class="sxs-lookup"><span data-stu-id="06413-187">Before creating a new ASP.NET MVC application, you should delete these files.</span></span> <span data-ttu-id="06413-188">您可以刪除檔案和資料夾，在 [方案總管] 視窗中的檔案或資料夾上按一下滑鼠右鍵，然後選取功能表選項**刪除**。</span><span class="sxs-lookup"><span data-stu-id="06413-188">You can delete files and folders in the Solution Explorer window by right-clicking a file or folder and selecting the menu option **Delete**.</span></span>

<span data-ttu-id="06413-189">您需要從 ASP.NET MVC 專案中刪除下列檔案：</span><span class="sxs-lookup"><span data-stu-id="06413-189">You need to delete the following files from the ASP.NET MVC project:</span></span>

- <span data-ttu-id="06413-190">\Controllers\HomeController.vb</span><span class="sxs-lookup"><span data-stu-id="06413-190">\Controllers\HomeController.vb</span></span>

- <span data-ttu-id="06413-191">\Views\Home\About.aspx</span><span class="sxs-lookup"><span data-stu-id="06413-191">\Views\Home\About.aspx</span></span>

- <span data-ttu-id="06413-192">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="06413-192">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="06413-193">而且，您需要從測試專案中刪除下列檔案：</span><span class="sxs-lookup"><span data-stu-id="06413-193">And, you need to delete the following file from the Test project:</span></span>

<span data-ttu-id="06413-194">\Controllers\HomeControllerTest.vb</span><span class="sxs-lookup"><span data-stu-id="06413-194">\Controllers\HomeControllerTest.vb</span></span>

## <a name="creating-the-database"></a><span data-ttu-id="06413-195">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="06413-195">Creating the Database</span></span>

<span data-ttu-id="06413-196">請連絡系統管理員應用程式是資料庫驅動的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-196">The Contact Manager application is a database-driven web application.</span></span> <span data-ttu-id="06413-197">我們會使用資料庫來儲存連絡資訊。</span><span class="sxs-lookup"><span data-stu-id="06413-197">We use a database to store the contact information.</span></span>

<span data-ttu-id="06413-198">ASP.NET MVC 架構，與任何最新的資料庫，包括 Microsoft SQL Server、 Oracle、 MySQL 和 IBM DB2 資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-198">The ASP.NET MVC framework with any modern database including Microsoft SQL Server, Oracle, MySQL, and IBM DB2 databases.</span></span> <span data-ttu-id="06413-199">在本教學課程中，我們會使用 Microsoft SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-199">In this tutorial, we use a Microsoft SQL Server database.</span></span> <span data-ttu-id="06413-200">當您安裝 Visual Studio 時，便會提供安裝 Microsoft SQL Server Express 是免費版本的 Microsoft SQL Server 資料庫的選項。</span><span class="sxs-lookup"><span data-stu-id="06413-200">When you install Visual Studio, you are provided with the option of installing Microsoft SQL Server Express which is a free version of the Microsoft SQL Server database.</span></span>

<span data-ttu-id="06413-201">以滑鼠右鍵按一下 應用程式建立新的資料庫\_在 方案總管 視窗，然後選取功能表選項中的 資料 資料夾**新增、 新項目**。</span><span class="sxs-lookup"><span data-stu-id="06413-201">Create a new database by right-clicking the App\_Data folder in the Solution Explorer window and selecting the menu option **Add, New Item**.</span></span> <span data-ttu-id="06413-202">在 **加入新項目**對話方塊中，選取**資料**類別目錄和**SQL Server 資料庫**範本 （請參閱 圖 4）。</span><span class="sxs-lookup"><span data-stu-id="06413-202">In the **Add New Item** dialog, select the **Data** category and the **SQL Server Database** template (see Figure 4).</span></span> <span data-ttu-id="06413-203">新的資料庫 ContactManagerDB.mdf 命名，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="06413-203">Name the new database ContactManagerDB.mdf and click the OK button.</span></span>


<span data-ttu-id="06413-204">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="06413-204">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)</span></span>

<span data-ttu-id="06413-205">**圖 04**： 建立新的 Microsoft SQL Server Express 資料庫 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="06413-205">**Figure 04**: Creating a new Microsoft SQL Server Express database ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image8.png))</span></span>


<span data-ttu-id="06413-206">建立新的資料庫之後，資料庫會出現在應用程式\_在 [方案總管] 視窗中的 [資料] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="06413-206">After you create the new database, the database appears in the App\_Data folder in the Solution Explorer window.</span></span> <span data-ttu-id="06413-207">按兩下 ContactManager.mdf 檔案以開啟 [伺服器總管] 視窗，並連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-207">Double-click the ContactManager.mdf file to open the Server Explorer window and connect to the database.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="06413-208">[伺服器總管] 視窗會呼叫在 Microsoft Visual Web Developer 的情況下的 [資料庫總管] 視窗。</span><span class="sxs-lookup"><span data-stu-id="06413-208">The Server Explorer window is called the Database Explorer window in the case of Microsoft Visual Web Developer.</span></span>


<span data-ttu-id="06413-209">您可以使用 [伺服器總管] 視窗來建立新的資料庫物件，例如資料庫資料表、 檢視、 觸發程序和預存程序。</span><span class="sxs-lookup"><span data-stu-id="06413-209">You can use the Server Explorer window to create new database objects such as database tables, views, triggers, and stored procedures.</span></span> <span data-ttu-id="06413-210">以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="06413-210">Right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="06413-211">資料庫資料表設計工具隨即出現 （請參閱 [圖 5]）。</span><span class="sxs-lookup"><span data-stu-id="06413-211">The Database Table Designer appears (see Figure 5).</span></span>


<span data-ttu-id="06413-212">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="06413-212">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)</span></span>

<span data-ttu-id="06413-213">**圖 05**： 資料庫資料表設計工具 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="06413-213">**Figure 05**: The Database Table Designer ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image10.png))</span></span>


<span data-ttu-id="06413-214">我們需要建立包含下列資料行的資料表：</span><span class="sxs-lookup"><span data-stu-id="06413-214">We need to create a table that contains the following columns:</span></span>

<a id="0.2_table01"></a>


| <span data-ttu-id="06413-215">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="06413-215">**Column Name**</span></span> | <span data-ttu-id="06413-216">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="06413-216">**Data Type**</span></span> | <span data-ttu-id="06413-217">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="06413-217">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="06413-218">ID</span><span class="sxs-lookup"><span data-stu-id="06413-218">Id</span></span> | <span data-ttu-id="06413-219">int</span><span class="sxs-lookup"><span data-stu-id="06413-219">int</span></span> | <span data-ttu-id="06413-220">False</span><span class="sxs-lookup"><span data-stu-id="06413-220">false</span></span> |
| <span data-ttu-id="06413-221">FirstName</span><span class="sxs-lookup"><span data-stu-id="06413-221">FirstName</span></span> | <span data-ttu-id="06413-222">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="06413-222">nvarchar(50)</span></span> | <span data-ttu-id="06413-223">False</span><span class="sxs-lookup"><span data-stu-id="06413-223">false</span></span> |
| <span data-ttu-id="06413-224">LastName</span><span class="sxs-lookup"><span data-stu-id="06413-224">LastName</span></span> | <span data-ttu-id="06413-225">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="06413-225">nvarchar(50)</span></span> | <span data-ttu-id="06413-226">False</span><span class="sxs-lookup"><span data-stu-id="06413-226">false</span></span> |
| <span data-ttu-id="06413-227">電話</span><span class="sxs-lookup"><span data-stu-id="06413-227">Phone</span></span> | <span data-ttu-id="06413-228">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="06413-228">nvarchar(50)</span></span> | <span data-ttu-id="06413-229">False</span><span class="sxs-lookup"><span data-stu-id="06413-229">false</span></span> |
| <span data-ttu-id="06413-230">Email</span><span class="sxs-lookup"><span data-stu-id="06413-230">Email</span></span> | <span data-ttu-id="06413-231">nvarchar(255)</span><span class="sxs-lookup"><span data-stu-id="06413-231">nvarchar(255)</span></span> | <span data-ttu-id="06413-232">False</span><span class="sxs-lookup"><span data-stu-id="06413-232">false</span></span> |


<span data-ttu-id="06413-233">第一個資料行，[識別碼] 欄中，是特殊的。</span><span class="sxs-lookup"><span data-stu-id="06413-233">The first column, the Id column, is special.</span></span> <span data-ttu-id="06413-234">您需要將識別碼資料行標示為識別資料行和主索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="06413-234">You need to mark the Id column as an Identity column and a Primary Key column.</span></span> <span data-ttu-id="06413-235">您指定的資料行是識別欄位的展開資料行屬性 （圖 6 底部查看） 並捲動到 識別規格 屬性。</span><span class="sxs-lookup"><span data-stu-id="06413-235">You indicate that a column is an Identity column by expanding Column Properites (look at the bottom of Figure 6) and scrolling down to the Identity Specification property.</span></span> <span data-ttu-id="06413-236">設定 **（為識別）** 屬性設為值**是**。</span><span class="sxs-lookup"><span data-stu-id="06413-236">Set the **(Is Identity)** property to the value **Yes**.</span></span>

<span data-ttu-id="06413-237">您將資料行標示為的主索引鍵資料行選取資料行，然後按一下 機碼的圖示的按鈕。</span><span class="sxs-lookup"><span data-stu-id="06413-237">You mark a column as a Primary Key column by selecting the column and clicking the button with the icon of a key.</span></span> <span data-ttu-id="06413-238">資料行標示為的主索引鍵資料行之後，資料行旁邊會出現索引鍵的圖示 （請參閱 圖 6）。</span><span class="sxs-lookup"><span data-stu-id="06413-238">After a column is marked as a Primary Key column, an icon of a key appears next to the column (see Figure 6).</span></span>

<span data-ttu-id="06413-239">完成 建立資料表之後，按一下 儲存 按鈕 （具有磁碟片圖示的按鈕） 以儲存新的資料表。</span><span class="sxs-lookup"><span data-stu-id="06413-239">After you finish creating the table, click the Save button (the button with an icon of a floppy) to save the new table.</span></span> <span data-ttu-id="06413-240">提供您新的資料表名稱*連絡人*。</span><span class="sxs-lookup"><span data-stu-id="06413-240">Give your new table the name *Contacts*.</span></span>

<span data-ttu-id="06413-241">完成 建立連絡人資料庫資料表之後, 您就應該在資料表中新增一些記錄。</span><span class="sxs-lookup"><span data-stu-id="06413-241">After finish creating the Contacts database table, you should add some records to the table.</span></span> <span data-ttu-id="06413-242">以滑鼠右鍵按一下 [伺服器總管] 視窗中的 [連絡人] 資料表，然後選取功能表選項**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="06413-242">Right-click the Contacts table in the Server Explorer window and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="06413-243">在出現的方格中輸入一或多個連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-243">Enter one or more contacts in the grid that appears.</span></span>

## <a name="creating-the-data-model"></a><span data-ttu-id="06413-244">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="06413-244">Creating the Data Model</span></span>

<span data-ttu-id="06413-245">ASP.NET MVC 應用程式是由模型、 檢視和控制站所組成。</span><span class="sxs-lookup"><span data-stu-id="06413-245">The ASP.NET MVC application consists of Models, Views, and Controllers.</span></span> <span data-ttu-id="06413-246">我們開始建立模型類別，表示我們在上一節中建立的 [連絡人] 資料表。</span><span class="sxs-lookup"><span data-stu-id="06413-246">We start by creating a Model class that represents the Contacts table that we created in the previous section.</span></span>

<span data-ttu-id="06413-247">在本教學課程中，我們會使用 Microsoft Entity Framework 自動從資料庫產生模型類別。</span><span class="sxs-lookup"><span data-stu-id="06413-247">In this tutorial, we use the Microsoft Entity Framework to generate a model class from the database automatically.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="06413-248">ASP.NET MVC 架構不會繫結至 Microsoft Entity Framework，以任何方式。</span><span class="sxs-lookup"><span data-stu-id="06413-248">The ASP.NET MVC framework is not tied to the Microsoft Entity Framework in any way.</span></span> <span data-ttu-id="06413-249">您可以使用 ASP.NET MVC 與替代的資料庫存取技術，包括 NHibernate、 LINQ to SQL 或 ADO.NET。</span><span class="sxs-lookup"><span data-stu-id="06413-249">You can use ASP.NET MVC with alternative database access technologies including NHibernate, LINQ to SQL, or ADO.NET.</span></span>


<span data-ttu-id="06413-250">請遵循下列步驟來建立資料模型類別：</span><span class="sxs-lookup"><span data-stu-id="06413-250">Follow these steps to create the data model classes:</span></span>

1. <span data-ttu-id="06413-251">在 [方案總管] 視窗中的 [模型] 資料夾上按一下滑鼠右鍵，然後選取**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="06413-251">Right-click the Models folder in the Solution Explorer window and select **Add, New Item**.</span></span> <span data-ttu-id="06413-252">**加入新項目**對話方塊隨即出現 （請參閱 圖 6）。</span><span class="sxs-lookup"><span data-stu-id="06413-252">The **Add New Item** dialog appears (see Figure 6).</span></span>
2. <span data-ttu-id="06413-253">選取 **資料**類別和**ADO.NET 實體資料模型**範本。</span><span class="sxs-lookup"><span data-stu-id="06413-253">Select the **Data** category and the **ADO.NET Entity Data Model** template.</span></span> <span data-ttu-id="06413-254">命名您的資料模型*ContactManagerModel.edmx*然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="06413-254">Name your data model *ContactManagerModel.edmx* and click the **Add** button.</span></span> <span data-ttu-id="06413-255">Entity Data Model 精靈 會出現 （請參閱 圖 7）。</span><span class="sxs-lookup"><span data-stu-id="06413-255">The Entity Data Model wizard appears (see Figure 7).</span></span>
3. <span data-ttu-id="06413-256">在 **選擇模型內容**步驟中，選取**從資料庫產生**（請參閱 圖 7）。</span><span class="sxs-lookup"><span data-stu-id="06413-256">In the **Choose Model Contents** step, select **Generate from database** (see Figure 7).</span></span>
4. <span data-ttu-id="06413-257">在 **選擇資料連接**步驟中，選取 ContactManagerDB.mdf 資料庫，然後輸入名稱*ContactManagerDBEntities* （請參閱 圖 8） 的實體連接設定。</span><span class="sxs-lookup"><span data-stu-id="06413-257">In the **Choose Your Data Connection** step, select the ContactManagerDB.mdf database and enter the name *ContactManagerDBEntities* for the Entity Connection Settings (see Figure 8).</span></span>
5. <span data-ttu-id="06413-258">在 **選擇您的資料庫物件**步驟中，選取核取方塊 （請參閱 圖 9） 的資料表。</span><span class="sxs-lookup"><span data-stu-id="06413-258">In the **Choose Your Database Objects** step, select the checkbox labeled Tables (see Figure 9).</span></span> <span data-ttu-id="06413-259">資料模型會包含您的資料庫 （不會只執行一個 [連絡人] 資料表） 中所包含的所有資料表。</span><span class="sxs-lookup"><span data-stu-id="06413-259">The data model will include all tables contained in your database (there is just one, the Contacts table).</span></span> <span data-ttu-id="06413-260">輸入的命名空間*模型*。</span><span class="sxs-lookup"><span data-stu-id="06413-260">Enter the namespace *Models*.</span></span> <span data-ttu-id="06413-261">按一下 [完成] 按鈕，以完成精靈。</span><span class="sxs-lookup"><span data-stu-id="06413-261">Click the Finish button to complete the wizard.</span></span>


<span data-ttu-id="06413-262">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="06413-262">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)</span></span>

<span data-ttu-id="06413-263">**圖 06**: 加入新項目對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="06413-263">**Figure 06**: The Add New Item dialog ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image12.png))</span></span>


<span data-ttu-id="06413-264">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="06413-264">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)</span></span>

<span data-ttu-id="06413-265">**圖 07**： 選擇模型內容 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="06413-265">**Figure 07**: Choose Model Contents ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image14.png))</span></span>


<span data-ttu-id="06413-266">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="06413-266">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)</span></span>

<span data-ttu-id="06413-267">**圖 08**： 選擇資料連接 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="06413-267">**Figure 08**: Choose Your Data Connection ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image16.png))</span></span>


<span data-ttu-id="06413-268">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="06413-268">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)</span></span>

<span data-ttu-id="06413-269">**圖 09**： 選擇您的資料庫物件 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="06413-269">**Figure 09**: Choose Your Database Objects ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image18.png))</span></span>


<span data-ttu-id="06413-270">Entity Data Model 精靈完成之後，實體資料模型設計工具就會出現。</span><span class="sxs-lookup"><span data-stu-id="06413-270">After you complete the Entity Data Model Wizard, the Entity Data Model Designer appears.</span></span> <span data-ttu-id="06413-271">設計工具會在建立模型的每個資料表顯示對應的類別。</span><span class="sxs-lookup"><span data-stu-id="06413-271">The designer displays a class that corresponds to each table being modeled.</span></span> <span data-ttu-id="06413-272">您應該會看到一個名為連絡人的類別。</span><span class="sxs-lookup"><span data-stu-id="06413-272">You should see one class named Contacts.</span></span>

<span data-ttu-id="06413-273">Entity Data Model 精靈 會產生以基礎資料庫資料表名稱的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="06413-273">The Entity Data Model wizard generates class names based on database table names.</span></span> <span data-ttu-id="06413-274">您幾乎都需要變更由精靈所產生之類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="06413-274">You almost always need to change the name of the class generated by the wizard.</span></span> <span data-ttu-id="06413-275">以滑鼠右鍵按一下設計工具中的 [連絡人] 類別，然後選取功能表選項**重新命名**。</span><span class="sxs-lookup"><span data-stu-id="06413-275">Right-click the Contacts class in the designer and select the menu option **Rename**.</span></span> <span data-ttu-id="06413-276">連絡人 （複數） 的類別名稱變更為連絡人 （單一） 中。</span><span class="sxs-lookup"><span data-stu-id="06413-276">Change the name of the class from Contacts (plural) to Contact (singular).</span></span> <span data-ttu-id="06413-277">變更類別名稱之後，類別應該會出現如 圖 10。</span><span class="sxs-lookup"><span data-stu-id="06413-277">After you change the class name, the class should appear like Figure 10.</span></span>


<span data-ttu-id="06413-278">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="06413-278">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)</span></span>

<span data-ttu-id="06413-279">**圖 10**： 連絡人類別 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="06413-279">**Figure 10**: The Contact class ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image20.png))</span></span>


<span data-ttu-id="06413-280">到目前為止，我們建立我們的資料庫模型。</span><span class="sxs-lookup"><span data-stu-id="06413-280">At this point, we have created our database model.</span></span> <span data-ttu-id="06413-281">我們可以使用連絡人類別來代表在資料庫中的特定的連絡人記錄。</span><span class="sxs-lookup"><span data-stu-id="06413-281">We can use the Contact class to represent a particular contact record in our database.</span></span>

## <a name="creating-the-home-controller"></a><span data-ttu-id="06413-282">建立主控制器</span><span class="sxs-lookup"><span data-stu-id="06413-282">Creating the Home Controller</span></span>

<span data-ttu-id="06413-283">下一個步驟是建立我們的首頁控制器。</span><span class="sxs-lookup"><span data-stu-id="06413-283">The next step is to create our Home controller.</span></span> <span data-ttu-id="06413-284">主控制器是在 ASP.NET MVC 應用程式中叫用的預設控制器。</span><span class="sxs-lookup"><span data-stu-id="06413-284">The Home controller is the default controller invoked in an ASP.NET MVC application.</span></span>

<span data-ttu-id="06413-285">在 方案總管 視窗中的 控制器 資料夾上按一下滑鼠右鍵，然後選取功能表選項來建立首頁控制器類別**新增、 控制站**（請參閱 圖 11）。</span><span class="sxs-lookup"><span data-stu-id="06413-285">Create the Home controller class by right-clicking the Controllers folder in the Solution Explorer window and selecting the menu option **Add, Controller** (see Figure 11).</span></span> <span data-ttu-id="06413-286">請注意標示的核取方塊**將動作方法，如 Create、 Update 和 Details 案例加入**。</span><span class="sxs-lookup"><span data-stu-id="06413-286">Notice the checkbox labeled **Add action methods for Create, Update, and Details scenarios**.</span></span> <span data-ttu-id="06413-287">請確定此核取方塊已選取後，再按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="06413-287">Make sure that this checkbox is checked before clicking the **Add** button.</span></span>


<span data-ttu-id="06413-288">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="06413-288">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)</span></span>

<span data-ttu-id="06413-289">**圖 11**： 加入主控制器 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="06413-289">**Figure 11**: Adding the Home controller ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image22.png))</span></span>


<span data-ttu-id="06413-290">當您建立主控制器時，您會取得列表 1 中的類別。</span><span class="sxs-lookup"><span data-stu-id="06413-290">When you create the Home controller, you get the class in Listing 1.</span></span>

<span data-ttu-id="06413-291">**列表 1-Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="06413-291">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a><span data-ttu-id="06413-292">列出連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-292">Listing the Contacts</span></span>

<span data-ttu-id="06413-293">若要顯示連絡人資料庫資料表中的記錄，我們要建立 index （） 動作和 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-293">In order to display the records in the Contacts database table, we need to create an Index() action and an Index view.</span></span>

<span data-ttu-id="06413-294">主控制器已經包含 index （） 動作。</span><span class="sxs-lookup"><span data-stu-id="06413-294">The Home controller already contains an Index() action.</span></span> <span data-ttu-id="06413-295">我們需要修改這個方法，讓它看起來像 列表 2。</span><span class="sxs-lookup"><span data-stu-id="06413-295">We need to modify this method so that it looks like Listing 2.</span></span>

<span data-ttu-id="06413-296">**列表 2-Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="06413-296">**Listing 2 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

<span data-ttu-id="06413-297">請注意，在 列表 2 中的首頁控制器類別包含名為私用欄位\_實體。</span><span class="sxs-lookup"><span data-stu-id="06413-297">Notice that the Home controller class in Listing 2 contains a private field named \_entities.</span></span> <span data-ttu-id="06413-298">\_實體欄位代表實體資料模型。</span><span class="sxs-lookup"><span data-stu-id="06413-298">The \_entities field represents the entities from the data model.</span></span> <span data-ttu-id="06413-299">我們使用\_實體欄位設為與資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="06413-299">We use the \_entities field to communicate with the database.</span></span>

<span data-ttu-id="06413-300">Index （） 方法會傳回所有連絡人代表連絡人資料庫資料表中的檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-300">The Index() method returns a view that represents all of the contacts from the Contacts database table.</span></span> <span data-ttu-id="06413-301">運算式\_實體。ContactSet.ToList() 傳回泛型清單的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="06413-301">The expression \_entities.ContactSet.ToList() returns the list of contacts as a generic list.</span></span>

<span data-ttu-id="06413-302">現在我們已建立索引控制器時，我們接下來要建立索引檢視表。</span><span class="sxs-lookup"><span data-stu-id="06413-302">Now that we ve created the Index controller, we next need to create the Index view.</span></span> <span data-ttu-id="06413-303">之前建立索引檢視時，請選取功能表選項，您的應用程式編譯**建置時，建置方案**。</span><span class="sxs-lookup"><span data-stu-id="06413-303">Before creating the Index view, compile your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="06413-304">您一律應該編譯專案，然後再將檢視新增模型類別的清單中顯示的順序**加入檢視**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="06413-304">You should always compile your project before adding a view in order for the list of model classes to be displayed in the **Add View** dialog.</span></span>

<span data-ttu-id="06413-305">您可以建立索引檢視的 index （） 方法上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱 圖 12）。</span><span class="sxs-lookup"><span data-stu-id="06413-305">You create the Index view by right-clicking the Index() method and selecting the menu option **Add View** (see Figure 12).</span></span> <span data-ttu-id="06413-306">選取此功能表選項會開啟**加入檢視**對話方塊 （請參閱 圖 13）。</span><span class="sxs-lookup"><span data-stu-id="06413-306">Selecting this menu option opens the **Add View** dialog (see Figure 13).</span></span>


<span data-ttu-id="06413-307">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="06413-307">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)</span></span>

<span data-ttu-id="06413-308">**圖 12**： 加入索引檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="06413-308">**Figure 12**: Adding the Index view ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image24.png))</span></span>


<span data-ttu-id="06413-309">在 [**加入檢視**] 對話方塊中，檢查標示的核取方塊**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="06413-309">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="06413-310">選取 檢視資料類別 ContactManager.Contact 和檢視內容清單。</span><span class="sxs-lookup"><span data-stu-id="06413-310">Select the View data class ContactManager.Contact and the View content List.</span></span> <span data-ttu-id="06413-311">選取這些選項會產生一個檢視，顯示一份連絡人記錄。</span><span class="sxs-lookup"><span data-stu-id="06413-311">Selecting these options generates a view that displays a list of Contact records.</span></span>


<span data-ttu-id="06413-312">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="06413-312">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)</span></span>

<span data-ttu-id="06413-313">**圖 13**: 新增檢視對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="06413-313">**Figure 13**: The Add View dialog ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image26.png))</span></span>


<span data-ttu-id="06413-314">當您按一下 **新增**產生按鈕，在 列表 3 中的 索引 檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-314">When you click the **Add** button, the Index view in Listing 3 is generated.</span></span> <span data-ttu-id="06413-315">請注意&lt;%@ %頁&gt;出現在檔案頂端的指示詞。</span><span class="sxs-lookup"><span data-stu-id="06413-315">Notice the &lt;%@ Page %&gt; directive that appears at the top of the file.</span></span> <span data-ttu-id="06413-316">[索引] 檢視繼承自 ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;類別。</span><span class="sxs-lookup"><span data-stu-id="06413-316">The Index view inherits from the ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt;&gt; class.</span></span> <span data-ttu-id="06413-317">換句話說，在檢視模型類別代表連絡人實體的清單。</span><span class="sxs-lookup"><span data-stu-id="06413-317">In other words, the Model class in the view represents a list of Contact entities.</span></span>

<span data-ttu-id="06413-318">[索引] 檢視的主體包含 foreach 迴圈，逐一查看每個模型類別所表示的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-318">The body of the Index view contains a foreach loop that iterates through each of the contacts represented by the Model class.</span></span> <span data-ttu-id="06413-319">連絡人類別的每個屬性的值會顯示在 HTML 表格。</span><span class="sxs-lookup"><span data-stu-id="06413-319">The value of each property of the Contact class is displayed within an HTML table.</span></span>

<span data-ttu-id="06413-320">**列表 3-Views\Home\Index.aspx （未經修改）**</span><span class="sxs-lookup"><span data-stu-id="06413-320">**Listing 3 - Views\Home\Index.aspx (unmodified)**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

<span data-ttu-id="06413-321">我們需要進行一項修改索引檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-321">We need to make one modification to the Index view.</span></span> <span data-ttu-id="06413-322">因為我們不會建立詳細資料檢視，我們可以移除詳細資料 連結。</span><span class="sxs-lookup"><span data-stu-id="06413-322">Because we are not creating a Details view, we can remove the Details link.</span></span> <span data-ttu-id="06413-323">尋找並移除索引檢視中的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="06413-323">Find and remove the following code from the Index view:</span></span>

<span data-ttu-id="06413-324">{.id = 項目。識別碼}） %&gt;</span><span class="sxs-lookup"><span data-stu-id="06413-324">{.id = item.Id})%&gt;</span></span>

<span data-ttu-id="06413-325">修改 [索引] 檢視之後，您可以執行的連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-325">After you modify the Index view, you can run the Contact Manager application.</span></span> <span data-ttu-id="06413-326">選取 [開始偵錯] 功能表選項偵錯，或直接按 F5。</span><span class="sxs-lookup"><span data-stu-id="06413-326">Select the menu option Debug, Start Debugging or simply press F5.</span></span> <span data-ttu-id="06413-327">第一次執行應用程式，您會看到對話方塊 [圖 14] 中。</span><span class="sxs-lookup"><span data-stu-id="06413-327">The first time you run the application, you get the dialog in Figure 14.</span></span> <span data-ttu-id="06413-328">選取的選項**修改 Web.config 檔以啟用偵錯**，按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="06413-328">Select the option **Modify the Web.config file to enable debugging** and click the OK button.</span></span>


<span data-ttu-id="06413-329">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="06413-329">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)</span></span>

<span data-ttu-id="06413-330">**圖 14**： 啟用偵錯 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image28.png))</span><span class="sxs-lookup"><span data-stu-id="06413-330">**Figure 14**: Enabling debugging ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image28.png))</span></span>


<span data-ttu-id="06413-331">[索引] 檢視會傳回預設值。</span><span class="sxs-lookup"><span data-stu-id="06413-331">The Index view is returned by default.</span></span> <span data-ttu-id="06413-332">這個檢視會列出所有的連絡人資料庫資料表中的資料 （請參閱 圖 15）。</span><span class="sxs-lookup"><span data-stu-id="06413-332">This view lists all of the data from the Contacts database table (see Figure 15).</span></span>


<span data-ttu-id="06413-333">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="06413-333">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)</span></span>

<span data-ttu-id="06413-334">**圖 15**: [索引] 檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="06413-334">**Figure 15**: The Index view ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image30.png))</span></span>


<span data-ttu-id="06413-335">請注意 索引 檢視包含標示為 建立新的檢視底部的連結。</span><span class="sxs-lookup"><span data-stu-id="06413-335">Notice that the Index view includes a link labeled Create New at the bottom of the view.</span></span> <span data-ttu-id="06413-336">在下一步 區段中，您將了解如何建立新的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-336">In the next section, you learn how to create new contacts.</span></span>

## <a name="creating-new-contacts"></a><span data-ttu-id="06413-337">建立新的連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-337">Creating New Contacts</span></span>

<span data-ttu-id="06413-338">若要讓使用者能夠建立新的連絡人，我們需要將兩個 create （） 動作新增至主控制器。</span><span class="sxs-lookup"><span data-stu-id="06413-338">To enable users to create new contacts, we need to add two Create() actions to the Home controller.</span></span> <span data-ttu-id="06413-339">我們需要建立一個 create （） 動作傳回的 HTML 表單建立新的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-339">We need to create one Create() action that returns an HTML form for creating a new contact.</span></span> <span data-ttu-id="06413-340">我們需要建立執行實際的資料庫插入新的連絡人的第二個 create （） 動作。</span><span class="sxs-lookup"><span data-stu-id="06413-340">We need to create a second Create() action that performs the actual database insert of the new contact.</span></span>

<span data-ttu-id="06413-341">我們需要加入至主控制器的新 create （） 方法都包含在 列表 4 中。</span><span class="sxs-lookup"><span data-stu-id="06413-341">The new Create() methods that we need to add to the Home controller are contained in Listing 4.</span></span>

<span data-ttu-id="06413-342">**列表 4-Controllers\HomeController.vb （與建立方法）**</span><span class="sxs-lookup"><span data-stu-id="06413-342">**Listing 4 - Controllers\HomeController.vb (with Create methods)**</span></span>

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

<span data-ttu-id="06413-343">可以使用 HTTP GET 叫用第一個 create （） 方法，而第二個 create （） 方法，可以只透過 HTTP POST 叫用。</span><span class="sxs-lookup"><span data-stu-id="06413-343">The first Create() method can be invoked with an HTTP GET while the second Create() method can be invoked only by an HTTP POST.</span></span> <span data-ttu-id="06413-344">換句話說，只有在將 HTML 表單張貼時，第二個 create （） 方法就可以叫用。</span><span class="sxs-lookup"><span data-stu-id="06413-344">In other words, the second Create() method can be invoked only when posting an HTML form.</span></span> <span data-ttu-id="06413-345">第一個 create （） 方法只會傳回包含用於建立新的連絡人的 HTML 表單的檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-345">The first Create() method simply returns a view that contains the HTML form for creating a new contact.</span></span> <span data-ttu-id="06413-346">第二個 create （） 方法更有趣的是： 它在資料庫中加入新的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-346">The second Create() method is much more interesting: it adds the new contact to the database.</span></span>

<span data-ttu-id="06413-347">請注意，第二個 create （） 方法已修改成接受連絡人類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="06413-347">Notice that the second Create() method has been modified to accept an instance of the Contact class.</span></span> <span data-ttu-id="06413-348">從 HTML 表單張貼的表單值會繫結至這個連絡人類別由 ASP.NET MVC 架構會自動。</span><span class="sxs-lookup"><span data-stu-id="06413-348">The form values posted from the HTML form are bound to this Contact class by the ASP.NET MVC framework automatically.</span></span> <span data-ttu-id="06413-349">建立 HTML 表單中的每個表單欄位會指派給連絡人參數的屬性。</span><span class="sxs-lookup"><span data-stu-id="06413-349">Each form field from the HTML Create form is assigned to a property of the Contact parameter.</span></span>

<span data-ttu-id="06413-350">請注意，連絡人參數使用 [繫結] 屬性裝飾。</span><span class="sxs-lookup"><span data-stu-id="06413-350">Notice that the Contact parameter is decorated with a [Bind] attribute.</span></span> <span data-ttu-id="06413-351">[繫結] 屬性用來從繫結中排除的連絡人識別碼屬性。</span><span class="sxs-lookup"><span data-stu-id="06413-351">The [Bind] attribute is used to exclude the Contact Id property from binding.</span></span> <span data-ttu-id="06413-352">因為 Id 屬性表示身分識別屬性，我們會不 t 想来設定的 Id 屬性。</span><span class="sxs-lookup"><span data-stu-id="06413-352">Because the Id property represents an Identity property, we don t want to set the Id property.</span></span>

<span data-ttu-id="06413-353">在 create （） 方法的主體，Entity Framework 來插入資料庫中的新的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-353">In the body of the Create() method, the Entity Framework is used to insert the new Contact into the database.</span></span> <span data-ttu-id="06413-354">新的連絡人加入一組現有的連絡人，並呼叫 savechanges （） 方法來將這些變更推送回基礎資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-354">The new Contact is added to the existing set of Contacts and the SaveChanges() method is called to push these changes back to the underlying database.</span></span>

<span data-ttu-id="06413-355">您可以產生 HTML 表單的其中一種兩個 create （） 方法上按一下滑鼠右鍵，然後選取功能表選項，即可建立新的連絡人**加入檢視**（請參閱 圖 16）。</span><span class="sxs-lookup"><span data-stu-id="06413-355">You can generate an HTML form for creating new Contacts by right-clicking either of the two Create() methods and selecting the menu option **Add View** (see Figure 16).</span></span>


<span data-ttu-id="06413-356">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="06413-356">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)</span></span>

<span data-ttu-id="06413-357">**圖 16**： 加入建立檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image32.png))</span><span class="sxs-lookup"><span data-stu-id="06413-357">**Figure 16**: Adding the Create view ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image32.png))</span></span>


<span data-ttu-id="06413-358">在 **加入檢視**對話方塊中，選取**ContactManager.Contact**類別和**建立**檢視內容的選項 （請參閱 圖 17）。</span><span class="sxs-lookup"><span data-stu-id="06413-358">In the **Add View** dialog, select the **ContactManager.Contact** class and the **Create** option for view content (see Figure 17).</span></span> <span data-ttu-id="06413-359">當您按一下 **新增**按鈕，檢視會自動產生的建立。</span><span class="sxs-lookup"><span data-stu-id="06413-359">When you click the **Add** button, a Create view is generated automatically.</span></span>


<span data-ttu-id="06413-360">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="06413-360">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)</span></span>

<span data-ttu-id="06413-361">**圖 17**： 看到 explode 頁面 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image34.png))</span><span class="sxs-lookup"><span data-stu-id="06413-361">**Figure 17**: Seeing a page explode ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image34.png))</span></span>


<span data-ttu-id="06413-362">建立檢視的每個連絡人類別的屬性包含表單欄位。</span><span class="sxs-lookup"><span data-stu-id="06413-362">The Create view contains form fields for each of the properties of the Contact class.</span></span> <span data-ttu-id="06413-363">建立檢視的程式碼會包含在 列表 5。</span><span class="sxs-lookup"><span data-stu-id="06413-363">The code for the Create view is included in Listing 5.</span></span>

<span data-ttu-id="06413-364">**列表 5-Views\Home\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="06413-364">**Listing 5 - Views\Home\Create.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

<span data-ttu-id="06413-365">修改 create （） 方法，並加入建立檢視之後，您可以執行連絡人管理員應用程式，並建立新的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-365">After you modify the Create() methods and add the Create view, you can run the Contact Manger application and create new contacts.</span></span> <span data-ttu-id="06413-366">按一下 **新建**出現在索引檢視中瀏覽至 建立 檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="06413-366">Click the **Create New** link that appears in the Index view to navigate to the Create view.</span></span> <span data-ttu-id="06413-367">您應該會看到 [圖 18] 檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-367">You should see the view in Figure 18.</span></span>


<span data-ttu-id="06413-368">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="06413-368">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)</span></span>

<span data-ttu-id="06413-369">**圖 18**: 建立檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image36.png))</span><span class="sxs-lookup"><span data-stu-id="06413-369">**Figure 18**: The Create View ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image36.png))</span></span>


## <a name="editing-contacts"></a><span data-ttu-id="06413-370">編輯連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-370">Editing Contacts</span></span>

<span data-ttu-id="06413-371">加入編輯連絡人的記錄功能是非常類似於新增的功能，讓您建立新的連絡人記錄。</span><span class="sxs-lookup"><span data-stu-id="06413-371">Adding the functionality for editing a contact record is very similar to adding the functionality for creating new contact records.</span></span> <span data-ttu-id="06413-372">首先，我們需要將兩個新的編輯方法新增至首頁控制器類別。</span><span class="sxs-lookup"><span data-stu-id="06413-372">First, we need to add two new Edit methods to the Home controller class.</span></span> <span data-ttu-id="06413-373">在 列表 6 中含有這些新的 edit （） 方法。</span><span class="sxs-lookup"><span data-stu-id="06413-373">These new Edit() methods are contained in Listing 6.</span></span>

<span data-ttu-id="06413-374">**列表 6-Controllers\HomeController.vb （具有編輯方法）**</span><span class="sxs-lookup"><span data-stu-id="06413-374">**Listing 6 - Controllers\HomeController.vb (with Edit methods)**</span></span>

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

<span data-ttu-id="06413-375">第一種 edit （） 方法會叫用 HTTP GET 作業。</span><span class="sxs-lookup"><span data-stu-id="06413-375">The first Edit() method is invoked by an HTTP GET operation.</span></span> <span data-ttu-id="06413-376">Id 參數會傳遞至這個方法表示正在編輯的連絡人記錄識別碼。</span><span class="sxs-lookup"><span data-stu-id="06413-376">An Id parameter is passed to this method which represents the Id of the contact record being edited.</span></span> <span data-ttu-id="06413-377">Entity Framework 用來擷取連絡人符合該識別碼。會傳回包含 HTML 表單以編輯記錄的檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-377">The Entity Framework is used to retrieve a contact that matches the Id. A view that contains an HTML form for editing a record is returned.</span></span>

<span data-ttu-id="06413-378">第二個 edit （） 方法會執行實際的更新到資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-378">The second Edit() method performs the actual update to the database.</span></span> <span data-ttu-id="06413-379">這個方法會接受連絡人類別的執行個體，做為參數。</span><span class="sxs-lookup"><span data-stu-id="06413-379">This method accepts an instance of the Contact class as a parameter.</span></span> <span data-ttu-id="06413-380">ASP.NET MVC 架構會繫結至表單欄位的編輯表單至這個類別會自動。</span><span class="sxs-lookup"><span data-stu-id="06413-380">The ASP.NET MVC framework binds the form fields from the Edit form to this class automatically.</span></span> <span data-ttu-id="06413-381">編輯的連絡人 （需要 Id 屬性的值） 時，請注意，您不會包含 [繫結] 屬性。</span><span class="sxs-lookup"><span data-stu-id="06413-381">Notice that you don t include the[Bind] attribute when editing a Contact (we need the value of the Id property).</span></span>

<span data-ttu-id="06413-382">Entity Framework 用來將修改過的連絡人儲存至資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-382">The Entity Framework is used to save the modified Contact to the database.</span></span> <span data-ttu-id="06413-383">必須從資料庫第一次擷取原始的連絡人。</span><span class="sxs-lookup"><span data-stu-id="06413-383">The original Contact must be retrieved from the database first.</span></span> <span data-ttu-id="06413-384">接下來，Entity Framework ApplyPropertyChanges() 方法呼叫以將變更的連絡人記錄。</span><span class="sxs-lookup"><span data-stu-id="06413-384">Next, the Entity Framework ApplyPropertyChanges() method is called to record the changes to the Contact.</span></span> <span data-ttu-id="06413-385">最後，Entity Framework savechanges （） 方法會呼叫來保存基礎資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="06413-385">Finally, the Entity Framework SaveChanges() method is called to persist the changes to the underlying database.</span></span>

<span data-ttu-id="06413-386">您可以產生包含編輯表單，以滑鼠右鍵按一下 edit （） 方法，然後選取 新增檢視 功能表選項的檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-386">You can generate the view that contains the Edit form by right-clicking the Edit() method and selecting the menu option Add View.</span></span> <span data-ttu-id="06413-387">在 新增檢視 對話方塊中，選取**ContactManager.Models.Contact**類別和**編輯**檢視內容 （請參閱 圖 19）。</span><span class="sxs-lookup"><span data-stu-id="06413-387">In the Add View dialog, select the **ContactManager.Models.Contact** class and the **Edit** view content (see Figure 19).</span></span>


<span data-ttu-id="06413-388">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="06413-388">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)</span></span>

<span data-ttu-id="06413-389">**圖 19**： 加入編輯檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image38.png))</span><span class="sxs-lookup"><span data-stu-id="06413-389">**Figure 19**: Adding an Edit View ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image38.png))</span></span>


<span data-ttu-id="06413-390">當您按一下 [新增] 按鈕時，會自動產生新的編輯檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-390">When you click the Add button, a new Edit view is generated automatically.</span></span> <span data-ttu-id="06413-391">產生的 HTML 表單包含欄位對應至每個連絡人類別 （請參閱列表 7） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="06413-391">The HTML form that is generated contains fields that correspond to each of the properties of the Contact class (see Listing 7).</span></span>

<span data-ttu-id="06413-392">**列表 7-Views\Home\Edit.aspx**</span><span class="sxs-lookup"><span data-stu-id="06413-392">**Listing 7 - Views\Home\Edit.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a><span data-ttu-id="06413-393">刪除連絡人</span><span class="sxs-lookup"><span data-stu-id="06413-393">Deleting Contacts</span></span>

<span data-ttu-id="06413-394">如果您想要刪除的連絡人，則您需要將兩個 delete （） 動作新增至首頁控制器類別。</span><span class="sxs-lookup"><span data-stu-id="06413-394">If you want to delete contacts then you need to add two Delete() actions to the Home controller class.</span></span> <span data-ttu-id="06413-395">第一個 delete （） 動作會顯示刪除確認表單。</span><span class="sxs-lookup"><span data-stu-id="06413-395">The first Delete() action displays a delete confirmation form.</span></span> <span data-ttu-id="06413-396">第二個 delete （） 動作會執行實際的刪除。</span><span class="sxs-lookup"><span data-stu-id="06413-396">The second Delete() action performs the actual delete.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="06413-397">之後，在反覆項目 #7 中，我們修改連絡人管理員，以支援 Ajax 刪除一個步驟。</span><span class="sxs-lookup"><span data-stu-id="06413-397">Later, in Iteration #7, we modify the Contact Manager so that it supports a one step Ajax delete.</span></span>


<span data-ttu-id="06413-398">在 列表 8 包含兩個新的 delete （） 方法。</span><span class="sxs-lookup"><span data-stu-id="06413-398">The two new Delete() methods are contained in Listing 8.</span></span>

<span data-ttu-id="06413-399">**列表 8-Controllers\HomeController.vb （刪除方法）**</span><span class="sxs-lookup"><span data-stu-id="06413-399">**Listing 8 - Controllers\HomeController.vb (Delete methods)**</span></span>

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

<span data-ttu-id="06413-400">第一個 delete （） 方法會傳回從資料庫刪除連絡人記錄的確認表單 （請參閱 Figure20）。</span><span class="sxs-lookup"><span data-stu-id="06413-400">The first Delete() method returns a confirmation form for deleting a contact record from the database (see Figure20).</span></span> <span data-ttu-id="06413-401">第二個 delete （） 方法會執行實際的刪除作業對資料庫。</span><span class="sxs-lookup"><span data-stu-id="06413-401">The second Delete() method performs the actual delete operation against the database.</span></span> <span data-ttu-id="06413-402">從資料庫擷取原始的連絡人之後，Entity Framework DeleteObject() 和 savechanges （） 方法會呼叫來執行資料庫刪除。</span><span class="sxs-lookup"><span data-stu-id="06413-402">After the original contact has been retrieved from the database, the Entity Framework DeleteObject() and SaveChanges() methods are called to perform the database delete.</span></span>


<span data-ttu-id="06413-403">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="06413-403">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)</span></span>

<span data-ttu-id="06413-404">**圖 20**： 刪除確認檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image40.png))</span><span class="sxs-lookup"><span data-stu-id="06413-404">**Figure 20**: The delete confirmation view ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image40.png))</span></span>


<span data-ttu-id="06413-405">我們需要修改 [索引] 檢視，使其包含用來刪除連絡人記錄 （請參閱圖 21） 的連結。</span><span class="sxs-lookup"><span data-stu-id="06413-405">We need to modify the Index view so that it contains a link for deleting contact records (see Figure 21).</span></span> <span data-ttu-id="06413-406">您需要將下列程式碼新增至相同的資料表儲存格，其中包含 [編輯] 連結：</span><span class="sxs-lookup"><span data-stu-id="06413-406">You need to add the following code to the same table cell that contains the Edit link:</span></span>

<span data-ttu-id="06413-407">{.id = 項目。識別碼}） %&gt;</span><span class="sxs-lookup"><span data-stu-id="06413-407">{.id = item.Id})%&gt;</span></span>


<span data-ttu-id="06413-408">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="06413-408">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)</span></span>

<span data-ttu-id="06413-409">**圖 21**： 索引檢視與編輯連結 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image42.png))</span><span class="sxs-lookup"><span data-stu-id="06413-409">**Figure 21**: Index view with Edit link ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image42.png))</span></span>


<span data-ttu-id="06413-410">接下來，我們需要建立刪除確認檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-410">Next, we need to create the delete confirmation view.</span></span> <span data-ttu-id="06413-411">以滑鼠右鍵按一下首頁控制器類別中的 delete （） 方法，然後選取 [新增檢視] 功能表選項。</span><span class="sxs-lookup"><span data-stu-id="06413-411">Right-click the Delete() method in the Home controller class and select the menu option Add View.</span></span> <span data-ttu-id="06413-412">新增檢視 對話方塊隨即出現 （請參閱 圖 22）。</span><span class="sxs-lookup"><span data-stu-id="06413-412">The Add View dialog appears (see Figure 22).</span></span>

<span data-ttu-id="06413-413">不同於清單中，建立、 和編輯檢視中，在 [新增檢視] 對話方塊不包含建立刪除檢視的選項。</span><span class="sxs-lookup"><span data-stu-id="06413-413">Unlike in the case of the List, Create, and Edit views, the Add View dialog does not contain an option to create a Delete view.</span></span> <span data-ttu-id="06413-414">相反地，選取**ContactManager.Models.Contact**資料類別和**空白**檢視內容。</span><span class="sxs-lookup"><span data-stu-id="06413-414">Instead, select the **ContactManager.Models.Contact** data class and the **Empty** view content.</span></span> <span data-ttu-id="06413-415">選取空的檢視內容的選項將需要在我們建立自己的檢視。</span><span class="sxs-lookup"><span data-stu-id="06413-415">Selecting the Empty view content option will require us to create the view ourselves.</span></span>


<span data-ttu-id="06413-416">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="06413-416">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)</span></span>

<span data-ttu-id="06413-417">**圖 22**： 加入刪除確認檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image44.png))</span><span class="sxs-lookup"><span data-stu-id="06413-417">**Figure 22**: Adding the delete confirmation view ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image44.png))</span></span>


<span data-ttu-id="06413-418">刪除檢視的內容包含在 列表 9。</span><span class="sxs-lookup"><span data-stu-id="06413-418">The content of the Delete view is contained in Listing 9.</span></span> <span data-ttu-id="06413-419">此檢視包含一份表單，以確認是否特定連絡人應該刪除 （請參閱圖 21）。</span><span class="sxs-lookup"><span data-stu-id="06413-419">This view contains a form that confirms whether or not a particular contact should be deleted (see Figure 21).</span></span>

<span data-ttu-id="06413-420">**列表 9-Views\Home\Delete.aspx**</span><span class="sxs-lookup"><span data-stu-id="06413-420">**Listing 9 - Views\Home\Delete.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a><span data-ttu-id="06413-421">變更預設的控制站的名稱</span><span class="sxs-lookup"><span data-stu-id="06413-421">Changing the Name of the Default Controller</span></span>

<span data-ttu-id="06413-422">它可能會對您造成困擾我們控制器類別的名稱，使用 連絡人名為 HomeController 類別。</span><span class="sxs-lookup"><span data-stu-id="06413-422">It might bother you that the name of our controller class for working with contacts is named the HomeController class.</span></span> <span data-ttu-id="06413-423">不成問題 t 控制器命名為 ContactController 嗎？</span><span class="sxs-lookup"><span data-stu-id="06413-423">Shouldn t the controller be named ContactController?</span></span>

<span data-ttu-id="06413-424">此問題很容易修正。</span><span class="sxs-lookup"><span data-stu-id="06413-424">This issue is easy enough to fix.</span></span> <span data-ttu-id="06413-425">首先，我們需要重構首頁控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="06413-425">First, we need to refactor the name of the Home controller.</span></span> <span data-ttu-id="06413-426">Visual Studio 程式碼編輯器中開啟 HomeController 類別，以滑鼠右鍵按一下類別的名稱並選取功能表選項**重新命名**。</span><span class="sxs-lookup"><span data-stu-id="06413-426">Open the HomeController class in the Visual Studio Code Editor, right click the name of the class and select the menu option **Rename**.</span></span> <span data-ttu-id="06413-427">選取此功能表選項會開啟 [重新命名] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="06413-427">Selecting this menu option opens the Rename dialog.</span></span>


<span data-ttu-id="06413-428">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="06413-428">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)</span></span>

<span data-ttu-id="06413-429">**圖 23**： 重構的控制器名稱 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image46.png))</span><span class="sxs-lookup"><span data-stu-id="06413-429">**Figure 23**: Refactoring a controller name ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image46.png))</span></span>


<span data-ttu-id="06413-430">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="06413-430">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)</span></span>

<span data-ttu-id="06413-431">**圖 24**： 使用 [重新命名] 對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image48.png))</span><span class="sxs-lookup"><span data-stu-id="06413-431">**Figure 24**: Using the Rename dialog ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image48.png))</span></span>


<span data-ttu-id="06413-432">如果您重新命名您控制器類別時，Visual Studio 會更新 [Views] 資料夾中資料夾的名稱。</span><span class="sxs-lookup"><span data-stu-id="06413-432">If you rename your controller class, Visual Studio will update the name of the folder in the Views folder as well.</span></span> <span data-ttu-id="06413-433">Visual Studio 會將 \Views\Home 資料夾重新命名為 \Views\Contact 資料夾。</span><span class="sxs-lookup"><span data-stu-id="06413-433">Visual Studio will rename the \Views\Home folder to the \Views\Contact folder.</span></span>

<span data-ttu-id="06413-434">進行這項變更之後，您的應用程式將不再有主控制器。</span><span class="sxs-lookup"><span data-stu-id="06413-434">After you make this change, your application will no longer have a Home controller.</span></span> <span data-ttu-id="06413-435">當您執行您的應用程式時，您將取得錯誤頁面中圖 25。</span><span class="sxs-lookup"><span data-stu-id="06413-435">When you run your application, you'll get the error page in Figure 25.</span></span>


<span data-ttu-id="06413-436">[![[新增專案] 對話方塊](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="06413-436">[![The New Project dialog box](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)</span></span>

<span data-ttu-id="06413-437">**圖 25**： 預設的控制站 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-vb/_static/image50.png))</span><span class="sxs-lookup"><span data-stu-id="06413-437">**Figure 25**: No default controller ([Click to view full-size image](iteration-1-create-the-application-vb/_static/image50.png))</span></span>


<span data-ttu-id="06413-438">我們需要更新使用連絡人控制器，而不是主控制器的 Global.asax 檔案中的預設路由。</span><span class="sxs-lookup"><span data-stu-id="06413-438">We need to update the default route in the Global.asax file to use the Contact controller instead of the Home controller.</span></span> <span data-ttu-id="06413-439">開啟 Global.asax 檔案並修改預設路由 （請參閱列表 10） 所使用的預設控制器。</span><span class="sxs-lookup"><span data-stu-id="06413-439">Open the Global.asax file and modify the default controller used by the default route (see Listing 10).</span></span>

<span data-ttu-id="06413-440">**列表 10-Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="06413-440">**Listing 10 - Global.asax.vb**</span></span>

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

<span data-ttu-id="06413-441">進行這些變更之後，請連絡管理員會正確執行。</span><span class="sxs-lookup"><span data-stu-id="06413-441">After you make these changes, the Contact Manager will run correctly.</span></span> <span data-ttu-id="06413-442">現在，它將連絡人控制器類別做為預設控制器。</span><span class="sxs-lookup"><span data-stu-id="06413-442">Now, it will use the Contact controller class as the default controller.</span></span>

## <a name="summary"></a><span data-ttu-id="06413-443">總結</span><span class="sxs-lookup"><span data-stu-id="06413-443">Summary</span></span>

<span data-ttu-id="06413-444">在這個第一個反覆項目中，我們建立基本的連絡人管理員應用程式中的最快的方式可能。</span><span class="sxs-lookup"><span data-stu-id="06413-444">In this first iteration, we created a basic Contact Manager application in the fastest way possible.</span></span> <span data-ttu-id="06413-445">我們採用 Visual Studio 自動產生我們的控制器和檢視的初始程式碼的優點。</span><span class="sxs-lookup"><span data-stu-id="06413-445">We took advantage of Visual Studio to generate the initial code for our controllers and views automatically.</span></span> <span data-ttu-id="06413-446">我們也利用 Entity Framework 自動產生我們的資料庫模型類別。</span><span class="sxs-lookup"><span data-stu-id="06413-446">We also took advantage of the Entity Framework to generate our database model classes automatically.</span></span>

<span data-ttu-id="06413-447">目前，我們可以列出、 建立、 編輯和刪除與連絡人管理員應用程式的連絡人記錄。</span><span class="sxs-lookup"><span data-stu-id="06413-447">Currently, we can list, create, edit, and delete contact records with the Contact Manager application.</span></span> <span data-ttu-id="06413-448">換句話說，我們可以執行的所有資料庫驅動的 web 應用程式所需的基本資料庫作業。</span><span class="sxs-lookup"><span data-stu-id="06413-448">In other words, we can perform all of the basic database operations required by a database-driven web application.</span></span>

<span data-ttu-id="06413-449">不幸的是，我們的應用程式會有一些問題。</span><span class="sxs-lookup"><span data-stu-id="06413-449">Unfortunately, our application has some problems.</span></span> <span data-ttu-id="06413-450">第一，我不太願意承認，不過，請連絡系統管理員應用程式不是最具吸引力的應用程式。</span><span class="sxs-lookup"><span data-stu-id="06413-450">First and I hesitate to admit this, the Contact Manager application is not the most attractive application.</span></span> <span data-ttu-id="06413-451">它需要一些設計工作。</span><span class="sxs-lookup"><span data-stu-id="06413-451">It needs some design work.</span></span> <span data-ttu-id="06413-452">在下一個反覆項目中，我們將探討如何變更預設檢視主版頁面和階層式樣式表，以改善應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="06413-452">In the next iteration, we'll look at how we can change the default view master page and cascading style sheet to improve the appearance of the application.</span></span>

<span data-ttu-id="06413-453">其次，我們尚未實作任何形式的驗證。</span><span class="sxs-lookup"><span data-stu-id="06413-453">Second, we have not implemented any form validation.</span></span> <span data-ttu-id="06413-454">例如，沒有要阻止您建立連絡人表單提交不需要輸入任何表單欄位的值。</span><span class="sxs-lookup"><span data-stu-id="06413-454">For example, there is nothing to prevent you from submitting the Create contact form without entering values for any of the form fields.</span></span> <span data-ttu-id="06413-455">此外，您可以輸入無效的電話號碼和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="06413-455">Furthermore, you can enter invalid phone numbers and email addresses.</span></span> <span data-ttu-id="06413-456">我們開始來處理反覆項目 #3 中的表單驗證的問題。</span><span class="sxs-lookup"><span data-stu-id="06413-456">We start to tackle the problem of form validation in iteration #3.</span></span>

<span data-ttu-id="06413-457">最後，以及最重要的是，請連絡系統管理員應用程式的目前反覆項目無法輕鬆地修改或維護。</span><span class="sxs-lookup"><span data-stu-id="06413-457">Finally, and most importantly, the current iteration of the Contact Manager application cannot be easily modified or maintained.</span></span> <span data-ttu-id="06413-458">比方說，資料庫存取邏輯是內建權限的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="06413-458">For example, the database access logic is baked right into the controller actions.</span></span> <span data-ttu-id="06413-459">這表示，我們無法修改資料存取程式碼，而不需修改我們控制站。</span><span class="sxs-lookup"><span data-stu-id="06413-459">This means that we cannot modify our data access code without modifying our controllers.</span></span> <span data-ttu-id="06413-460">在後期反覆項目，我們會探討我們可以實作以進行連絡管理員更有彈性，若要變更的軟體設計模式。</span><span class="sxs-lookup"><span data-stu-id="06413-460">In later iterations, we explore software design patterns that we can implement to make the Contact Manager more resilient to change.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06413-461">[上一頁](iteration-7-add-ajax-functionality-cs.md)
> [下一頁](iteration-2-make-the-application-look-nice-vb.md)</span><span class="sxs-lookup"><span data-stu-id="06413-461">[Previous](iteration-7-add-ajax-functionality-cs.md)
[Next](iteration-2-make-the-application-look-nice-vb.md)</span></span>
