---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: "編輯程式碼的 Visual Studio 2013 中的 ASP.NET Web Form |Microsoft 文件"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: dfcddb4373fbf17ca29c5ab94c6ab3387ed6b526
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="f24b7-102">Visual Studio 2013 中的程式碼編輯的 ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="f24b7-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="f24b7-103">由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="f24b7-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="f24b7-104">在許多的 ASP.NET Web Form 頁面中，您可以撰寫 Visual Basic、 C# 中，或另一種語言中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f24b7-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="f24b7-105">在 Visual Studio 中的程式碼編輯器可協助您快速地撰寫程式碼，同時又能夠協助您避免錯誤。</span><span class="sxs-lookup"><span data-stu-id="f24b7-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="f24b7-106">此外，編輯器會提供方法讓您建立可重複使用程式碼以減少您需要執行的工作量。</span><span class="sxs-lookup"><span data-stu-id="f24b7-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="f24b7-107">這個逐步解說將說明 Visual Studio 程式碼編輯器的各種功能。</span><span class="sxs-lookup"><span data-stu-id="f24b7-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="f24b7-108">在這個逐步解說期間，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="f24b7-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="f24b7-109">修正內嵌程式碼撰寫錯誤。</span><span class="sxs-lookup"><span data-stu-id="f24b7-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="f24b7-110">重構和重新命名程式碼。</span><span class="sxs-lookup"><span data-stu-id="f24b7-110">Refactor and rename code.</span></span>
- <span data-ttu-id="f24b7-111">重新命名的變數和物件。</span><span class="sxs-lookup"><span data-stu-id="f24b7-111">Rename variables and objects.</span></span>
- <span data-ttu-id="f24b7-112">插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f24b7-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f24b7-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="f24b7-113">Prerequisites</span></span>


<span data-ttu-id="f24b7-114">若要完成這個逐步解說，您將需要：</span><span class="sxs-lookup"><span data-stu-id="f24b7-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="f24b7-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span> <span data-ttu-id="f24b7-116">會自動安裝.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="f24b7-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="f24b7-117">Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常被稱為 Visual Studio 在此教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="f24b7-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="f24b7-118">如果您使用 Visual Studio，本逐步解說假設您已經選擇**Web 程式開發**設定的集合，您會啟動 Visual Studio 的第一次。</span><span class="sxs-lookup"><span data-stu-id="f24b7-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="f24b7-119">如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

 <span data-ttu-id="f24b7-120">如需 Visual Studio 和 ASP.NET 的簡介，請參閱[在 Visual Studio 2013 中建立基本的 ASP.NET 4.5 Web Form 頁面](creating-a-basic-web-forms-page.md)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="f24b7-121">建立 Web 應用程式專案和頁面</span><span class="sxs-lookup"><span data-stu-id="f24b7-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="f24b7-122">在這部分的逐步解說中，您將建立 Web 應用程式專案，並加入新的頁面。</span><span class="sxs-lookup"><span data-stu-id="f24b7-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="f24b7-123">若要建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="f24b7-123">To create a Web application project</span></span>

1. <span data-ttu-id="f24b7-124">開啟 Microsoft Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f24b7-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="f24b7-125">在 [檔案] 功能表上，選取 [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="f24b7-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="f24b7-126">![檔案功能表](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f24b7-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="f24b7-127">[ **新增專案** ] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="f24b7-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="f24b7-128">選取**範本** - &gt; **Visual C#**  - &gt; **Web**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="f24b7-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="f24b7-129">選擇**ASP.NET Web 應用程式**中心資料行中的範本。</span><span class="sxs-lookup"><span data-stu-id="f24b7-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="f24b7-130">命名您的專案***BasicWebApp***按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f24b7-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="f24b7-131">![新增專案對話方塊](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="f24b7-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="f24b7-132">接下來，選取**Web Form**範本，然後按一下**確定**按鈕以建立專案。</span><span class="sxs-lookup"><span data-stu-id="f24b7-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="f24b7-133">![新增 ASP.NET 專案對話方塊](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f24b7-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="f24b7-134">Visual Studio 建立新的專案，其中包含預先建置的 Web Form 範本為基礎的功能。</span><span class="sxs-lookup"><span data-stu-id="f24b7-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="f24b7-135">建立新的 ASP.NET Web Form 網頁</span><span class="sxs-lookup"><span data-stu-id="f24b7-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="f24b7-136">當您建立新的 Web Form 應用程式使用**ASP.NET Web 應用程式**專案範本，Visual Studio 加入 ASP.NET 網頁 （Web Form 頁面） 名為*Default.aspx*，以及以多個其他檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="f24b7-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="f24b7-137">您可以使用*Default.aspx*與 Web 應用程式的 [首頁] 頁面的頁面。</span><span class="sxs-lookup"><span data-stu-id="f24b7-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="f24b7-138">不過，對於此逐步解說中，您會建立和使用新的頁面。</span><span class="sxs-lookup"><span data-stu-id="f24b7-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="f24b7-139">若要將頁面加入至 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f24b7-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="f24b7-140">在**方案總管 中**，以滑鼠右鍵按一下 Web 應用程式名稱 (在此教學課程中的應用程式名稱是**BasicWebSite**)，然後按一下**新增** - &gt;**新項目**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="f24b7-141">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f24b7-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="f24b7-142">選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="f24b7-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="f24b7-143">然後，選取**Web Form**中間清單並將其命名*FirstWebPage.aspx*。</span><span class="sxs-lookup"><span data-stu-id="f24b7-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="f24b7-144">![加入新項目對話方塊](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f24b7-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="f24b7-145">按一下**新增**Web Form 網頁加入您的專案。</span><span class="sxs-lookup"><span data-stu-id="f24b7-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="f24b7-146">Visual Studio 會建立新的頁面，並開啟它。</span><span class="sxs-lookup"><span data-stu-id="f24b7-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="f24b7-147">接下來，將這個新的頁面設定為預設的起始頁。</span><span class="sxs-lookup"><span data-stu-id="f24b7-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="f24b7-148">在**方案總管 中**，以滑鼠右鍵按一下名為新頁面*FirstWebPage.aspx*選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="f24b7-149">下次您執行此應用程式來測試進度，您會自動看到這個新的網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="f24b7-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="f24b7-150">修正內嵌程式碼錯誤</span><span class="sxs-lookup"><span data-stu-id="f24b7-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="f24b7-151">在 Visual Studio 中的程式碼編輯器，可協助您避免錯誤，因為您撰寫程式碼，以及如果您做了錯誤，程式碼編輯器可協助您更正這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="f24b7-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="f24b7-152">在這個部分的逐步解說中，您將撰寫說明錯誤修正功能，在編輯器中的程式碼行。</span><span class="sxs-lookup"><span data-stu-id="f24b7-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="f24b7-153">若要修正 Visual Studio 中的簡單程式碼錯誤</span><span class="sxs-lookup"><span data-stu-id="f24b7-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="f24b7-154">在**設計**檢視中，按兩下空白網頁，以建立的處理常式**負載**網頁事件。</span><span class="sxs-lookup"><span data-stu-id="f24b7-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
<span data-ttu-id="f24b7-155">您使用只做為位置的事件處理常式撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="f24b7-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="f24b7-156">內部處理常式中，輸入下列行，其中包含錯誤和按**ENTER**:</span><span class="sxs-lookup"><span data-stu-id="f24b7-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

 <span data-ttu-id="f24b7-157">當您按**ENTER**，程式碼編輯器中放置綠色及紅色底線 (通常呼叫&quot;曲線&quot;行) 下的程式碼有問題的區域。</span><span class="sxs-lookup"><span data-stu-id="f24b7-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="f24b7-158">綠色的底線表示警告。</span><span class="sxs-lookup"><span data-stu-id="f24b7-158">A green underline indicates a warning.</span></span> <span data-ttu-id="f24b7-159">紅色底線指出您必須修正的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f24b7-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="f24b7-160">將滑鼠指標`myStr`查看會告訴您有關警告的工具提示。</span><span class="sxs-lookup"><span data-stu-id="f24b7-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="f24b7-161">此外，請將滑鼠指標上的紅色底線，以查看錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f24b7-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="f24b7-162">下圖顯示的程式碼以底線表示。</span><span class="sxs-lookup"><span data-stu-id="f24b7-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="f24b7-163">![在 [設計] 檢視中的歡迎文字](code-editing-in-web-forms-pages/_static/image5.png "設計 檢視中的歡迎文字")</span><span class="sxs-lookup"><span data-stu-id="f24b7-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
 <span data-ttu-id="f24b7-164">必須先修正錯誤，加上分號`;`到一行的結尾。</span><span class="sxs-lookup"><span data-stu-id="f24b7-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="f24b7-165">警告只是通知您您未使用`myStr`尚未變數。</span><span class="sxs-lookup"><span data-stu-id="f24b7-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="f24b7-166">檢視您目前的程式碼格式化選取的 Visual Studio 中的設定**工具** - &gt; **選項** - &gt; **字型和色彩**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="f24b7-167">重構和重新命名</span><span class="sxs-lookup"><span data-stu-id="f24b7-167">Refactoring and Renaming</span></span>

<span data-ttu-id="f24b7-168">重構是一種軟體方法，包括重新建構您的程式碼，以便更容易了解和維護，同時保留其功能。</span><span class="sxs-lookup"><span data-stu-id="f24b7-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="f24b7-169">簡單的範例可能是您要從資料庫取得資料的事件處理常式中撰寫程式碼。</span><span class="sxs-lookup"><span data-stu-id="f24b7-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="f24b7-170">開發網頁時，會發現您需要從多個不同的處理常式存取資料。</span><span class="sxs-lookup"><span data-stu-id="f24b7-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="f24b7-171">因此，您會藉由在網頁中建立資料存取方法的處理常式中插入方法的呼叫，來重構網頁的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f24b7-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="f24b7-172">程式碼編輯器中包含工具，可協助您執行各種重構的工作。</span><span class="sxs-lookup"><span data-stu-id="f24b7-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="f24b7-173">在本逐步解說中，您將使用兩個重構技術： 重新命名變數和擷取方法。</span><span class="sxs-lookup"><span data-stu-id="f24b7-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="f24b7-174">重構的其他選項包括封裝欄位、 將區域變數提升為方法參數，以及管理方法參數。</span><span class="sxs-lookup"><span data-stu-id="f24b7-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="f24b7-175">這些重構選項的可用性取決於程式碼中的位置。</span><span class="sxs-lookup"><span data-stu-id="f24b7-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="f24b7-176">重構程式碼</span><span class="sxs-lookup"><span data-stu-id="f24b7-176">Refactoring Code</span></span>

<span data-ttu-id="f24b7-177">重構的常見案例是建立 （擷取） 位於另一個成員，例如方法的程式碼中的方法。</span><span class="sxs-lookup"><span data-stu-id="f24b7-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="f24b7-178">這會減少原始成員的大小，並使擷取的程式碼可重複使用。</span><span class="sxs-lookup"><span data-stu-id="f24b7-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="f24b7-179">在這部分的逐步解說中，您將撰寫一些簡單的程式碼，然後從其中擷取方法。</span><span class="sxs-lookup"><span data-stu-id="f24b7-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="f24b7-180">重構被支援 C# 中，因此您將建立使用 C# 做為程式設計語言的網頁。</span><span class="sxs-lookup"><span data-stu-id="f24b7-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="f24b7-181">若要擷取在 C# 頁面方法</span><span class="sxs-lookup"><span data-stu-id="f24b7-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="f24b7-182">切換至**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="f24b7-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="f24b7-183">在**工具箱**，從**標準**索引標籤，拖曳[按鈕](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.aspx)拖曳到頁面的控制項。</span><span class="sxs-lookup"><span data-stu-id="f24b7-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="f24b7-184">按兩下**按鈕**控制項建立的處理常式其[按一下](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.click.aspx)事件，然後加入下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f24b7-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

 <span data-ttu-id="f24b7-185">程式碼會建立**ArrayList**物件，若要載入的值，會使用迴圈，然後再使用另一個迴圈顯示的內容**ArrayList**物件。</span><span class="sxs-lookup"><span data-stu-id="f24b7-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="f24b7-186">按**CTRL + F5**來執行網頁，然後按一下**按鈕**先確定您會看到下列輸出：</span><span class="sxs-lookup"><span data-stu-id="f24b7-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="f24b7-187">返回程式碼編輯器中，然後選取下列幾行的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="f24b7-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="f24b7-188">以滑鼠右鍵按一下選取範圍中，按一下**重構**，然後選擇 **擷取方法**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="f24b7-189">**擷取方法** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="f24b7-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="f24b7-190">在**新方法名稱**方塊中，輸入**DisplayArray**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="f24b7-191">程式碼編輯器中建立新的方法，名為`DisplayArray`，並放入新的方法中呼叫**按一下**其中迴圈原本的處理常式。</span><span class="sxs-lookup"><span data-stu-id="f24b7-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="f24b7-192">按**CTRL + F5**執行頁面，並且按一下**按鈕**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="f24b7-193">網頁的運作與以前一樣。</span><span class="sxs-lookup"><span data-stu-id="f24b7-193">The page functions the same as it did before.</span></span> <span data-ttu-id="f24b7-194">`DisplayArray`方法現在可以從任何地方呼叫頁面類別中。</span><span class="sxs-lookup"><span data-stu-id="f24b7-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="f24b7-195">重新命名的變數</span><span class="sxs-lookup"><span data-stu-id="f24b7-195">Renaming Variables</span></span>

<span data-ttu-id="f24b7-196">當您使用變數，以及物件時，您可以之後在程式碼中參考重新命名。</span><span class="sxs-lookup"><span data-stu-id="f24b7-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="f24b7-197">不過，重新命名的變數和物件可能會導致程式碼中斷如果您未重新命名其中一個參考。</span><span class="sxs-lookup"><span data-stu-id="f24b7-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="f24b7-198">因此，您可以使用重構執行重新命名。</span><span class="sxs-lookup"><span data-stu-id="f24b7-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="f24b7-199">若要使用重構為變數重新命名</span><span class="sxs-lookup"><span data-stu-id="f24b7-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="f24b7-200">在**按一下**事件處理常式中，找出下列這一行：</span><span class="sxs-lookup"><span data-stu-id="f24b7-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="f24b7-201">以滑鼠右鍵按一下變數名稱`alist`，選擇**重構**，然後選擇 **重新命名**。</span><span class="sxs-lookup"><span data-stu-id="f24b7-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="f24b7-202">**重新命名** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="f24b7-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="f24b7-203">在**新名稱**方塊中，輸入**ArrayList1** ，並確定**預覽參考變更**已選取核取方塊。</span><span class="sxs-lookup"><span data-stu-id="f24b7-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="f24b7-204">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="f24b7-204">Then click **OK**.</span></span>

    <span data-ttu-id="f24b7-205">**預覽變更** 對話方塊隨即出現，並顯示包含您要重新命名該變數的所有參考的樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="f24b7-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="f24b7-206">按一下**套用**關閉**預覽變更** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f24b7-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="f24b7-207">特別是指您所選取的執行個體的變數會重新命名。</span><span class="sxs-lookup"><span data-stu-id="f24b7-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="f24b7-208">不過請注意，變數`alist`下列行中不會重新命名。</span><span class="sxs-lookup"><span data-stu-id="f24b7-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="f24b7-209">變數`alist`此行中不重新命名，因為它不代表相同的變數值`alist`已重新命名。</span><span class="sxs-lookup"><span data-stu-id="f24b7-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="f24b7-210">變數`alist`中`DisplayArray`宣告是針對該方法的本機變數。</span><span class="sxs-lookup"><span data-stu-id="f24b7-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="f24b7-211">這說明使用重構重新命名變數不同於直接在編輯器中，執行尋找和取代動作了解正在使用該變數的語意的重構重新命名變數。</span><span class="sxs-lookup"><span data-stu-id="f24b7-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="f24b7-212">插入程式碼片段</span><span class="sxs-lookup"><span data-stu-id="f24b7-212">Inserting Snippets</span></span>

<span data-ttu-id="f24b7-213">有許多 Web Form 開發人員經常需要執行的程式碼撰寫工作，因為程式碼編輯器中提供程式碼片段或預先撰寫的程式碼區塊的程式的庫。</span><span class="sxs-lookup"><span data-stu-id="f24b7-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="f24b7-214">您可以將這些程式碼片段插入您的頁面。</span><span class="sxs-lookup"><span data-stu-id="f24b7-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="f24b7-215">您使用 Visual Studio 中的每種語言有插入程式碼片段的方式有些微差異。</span><span class="sxs-lookup"><span data-stu-id="f24b7-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="f24b7-216">插入程式碼片段的相關資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx).</span></span> <span data-ttu-id="f24b7-217">如需插入程式碼片段中 Visual C# 中的資訊，請參閱[Visual C# 程式碼片段](https://msdn.microsoft.com/en-us/library/z41h7fat.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/en-us/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f24b7-218">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f24b7-218">Next Steps</span></span>

<span data-ttu-id="f24b7-219">本逐步解說已說明 Visual Studio 2010 程式碼編輯器的基本功能的程式碼中的錯誤、 重構程式碼、 重新命名變數和程式碼片段插入程式碼。</span><span class="sxs-lookup"><span data-stu-id="f24b7-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="f24b7-220">在編輯器中的其他功能可以確保應用程式開發快速又簡單。</span><span class="sxs-lookup"><span data-stu-id="f24b7-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="f24b7-221">例如，您可能要：</span><span class="sxs-lookup"><span data-stu-id="f24b7-221">For example, you might want to:</span></span>

- <span data-ttu-id="f24b7-222">進一步了解 IntelliSense，例如修改 IntelliSense 選項、 管理程式碼片段，以及搜尋線上程式碼片段的功能。</span><span class="sxs-lookup"><span data-stu-id="f24b7-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="f24b7-223">如需詳細資訊，請參閱[使用 IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="f24b7-224">了解如何建立您自己的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f24b7-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="f24b7-225">如需詳細資訊，請參閱[建立和使用 IntelliSense 程式碼片段](https://msdn.microsoft.com/en-us/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="f24b7-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/en-us/library/ms165392.aspx)</span></span>
- <span data-ttu-id="f24b7-226">深入了解 Visual Basic 特定的 IntelliSense 程式碼片段，例如自訂程式碼片段，以及疑難排解功能。</span><span class="sxs-lookup"><span data-stu-id="f24b7-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="f24b7-227">如需詳細資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="f24b7-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="f24b7-228">深入了解 C# 的特定功能的 IntelliSense，例如重構和程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f24b7-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="f24b7-229">如需詳細資訊，請參閱[Visual C# IntelliSense](https://msdn.microsoft.com/en-us/library/43f44291.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f24b7-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/en-us/library/43f44291.aspx).</span></span>
