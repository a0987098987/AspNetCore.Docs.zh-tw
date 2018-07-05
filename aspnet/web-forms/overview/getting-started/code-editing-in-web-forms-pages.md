---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: 程式碼編輯 Visual Studio 2013 中的 ASP.NET Web Form |Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 322a9aa6fb366eb9d5be33838fd6ac7ea51047f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371790"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="fd8c4-102">Visual Studio 2013 中的程式碼編輯 ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="fd8c4-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="fd8c4-103">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="fd8c4-104">在許多的 ASP.NET Web Form 頁面中，您可以撰寫 Visual Basic、 C# 中，或另一種語言中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="fd8c4-105">在 Visual Studio 中的程式碼編輯器可協助您快速撰寫程式碼，同時協助您避免錯誤。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="fd8c4-106">此外，編輯器會提供方法讓您建立可重複使用的程式碼以減少您需要執行的工作量。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="fd8c4-107">此逐步解說將說明 Visual Studio 程式碼編輯器的各種功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="fd8c4-108">在這個逐步解說期間，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="fd8c4-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="fd8c4-109">修正內嵌程式碼錯誤。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="fd8c4-110">重構和重新命名程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-110">Refactor and rename code.</span></span>
- <span data-ttu-id="fd8c4-111">重新命名的變數和物件。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-111">Rename variables and objects.</span></span>
- <span data-ttu-id="fd8c4-112">插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd8c4-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="fd8c4-113">Prerequisites</span></span>


<span data-ttu-id="fd8c4-114">若要完成這個逐步解說，您將需要：</span><span class="sxs-lookup"><span data-stu-id="fd8c4-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="fd8c4-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或是[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="fd8c4-116">.NET Framework 會自動安裝。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="fd8c4-117">Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常稱為 Visual Studio 在此教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="fd8c4-118">如果您使用 Visual Studio，本逐步解說假設您已經選擇**Web 開發**設定集合，第一次您啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="fd8c4-119">如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

  <span data-ttu-id="fd8c4-120">Visual Studio 及 ASP.NET 的簡介，請參閱 < [Visual Studio 2013 中建立基本的 ASP.NET 4.5 Web Form 頁面](creating-a-basic-web-forms-page.md)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="fd8c4-121">建立 Web 應用程式專案和頁面</span><span class="sxs-lookup"><span data-stu-id="fd8c4-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="fd8c4-122">在本逐步解說的這個部分，您會建立 Web 應用程式專案，並加入新的頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="fd8c4-123">若要建立的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="fd8c4-123">To create a Web application project</span></span>

1. <span data-ttu-id="fd8c4-124">開啟 Microsoft Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="fd8c4-125">在 [檔案] 功能表上，選取 [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="fd8c4-126">![[檔案] 功能表](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="fd8c4-127">[ **新增專案** ] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="fd8c4-128">選取 **範本** - &gt; **Visual C#**  - &gt; **Web**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="fd8c4-129">選擇**ASP.NET Web 應用程式**中間欄的範本。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="fd8c4-130">命名您的專案***BasicWebApp***然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="fd8c4-131">![[新增專案] 對話方塊](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="fd8c4-132">接下來，選取**Web Form**範本，然後按一下**確定**按鈕，以建立專案。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="fd8c4-133">![[新增 ASP.NET 專案] 對話方塊](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="fd8c4-134">Visual Studio 會建立新的專案，其中包含預先建置的 Web Form 範本為基礎的功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="fd8c4-135">建立新的 ASP.NET Web Form 頁面</span><span class="sxs-lookup"><span data-stu-id="fd8c4-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="fd8c4-136">當您建立新的 Web Form 應用程式使用**ASP.NET Web 應用程式**專案範本，Visual Studio 會加入名為 ASP.NET 網頁 （Web Form 頁面） *Default.aspx*，以及數個其他檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="fd8c4-137">您可以使用*Default.aspx*與 Web 應用程式的 [首頁] 頁面的頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="fd8c4-138">不過，對於此逐步解說中，您會建立並使用新的頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="fd8c4-139">將頁面新增至 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fd8c4-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="fd8c4-140">在**方案總管**，以滑鼠右鍵按一下 Web 應用程式名稱 (在應用程式名稱是本教學課程**BasicWebSite**)，然後按一下 **新增** - &gt;**新的項目**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="fd8c4-141">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="fd8c4-142">選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="fd8c4-143">然後，選取**Web Form**從中間清單並將它命名*FirstWebPage.aspx*。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="fd8c4-144">![加入新項目對話方塊](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="fd8c4-145">按一下 **新增**將 Web Form 頁面新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="fd8c4-146">Visual Studio 會建立新的頁面，並開啟它。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="fd8c4-147">接下來，將這個新頁面設定為預設起始頁中。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="fd8c4-148">在 [**方案總管] 中**，以滑鼠右鍵按一下名為新的頁面*FirstWebPage.aspx* ，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="fd8c4-149">下次您執行此應用程式來測試我們的進度，您會自動看到這個新的瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="fd8c4-150">修正內嵌程式碼錯誤</span><span class="sxs-lookup"><span data-stu-id="fd8c4-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="fd8c4-151">在 Visual Studio 中的程式碼編輯器，可協助您避免發生錯誤，因為您撰寫程式碼，而且如果您已發生錯誤，程式碼編輯器可協助您更正錯誤。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="fd8c4-152">在這部分的逐步解說中，您將撰寫一行程式碼說明在編輯器中的錯誤修正功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="fd8c4-153">若要修正 Visual Studio 中的簡單程式碼撰寫錯誤</span><span class="sxs-lookup"><span data-stu-id="fd8c4-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="fd8c4-154">在 **設計**檢視中，按兩下 空白的頁面，即可建立的處理常式**負載**事件頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="fd8c4-155">您只將事件處理常式當做位置，以撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="fd8c4-156">內部處理常式中，輸入下列行，其中包含錯誤，並按下**ENTER**:</span><span class="sxs-lookup"><span data-stu-id="fd8c4-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="fd8c4-157">當您按下**ENTER**，程式碼編輯器會在放置綠色和紅色底線 (通常會呼叫&quot;曲線&quot;行) 下的程式碼有問題的區域。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="fd8c4-158">綠色的底線表示警告。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-158">A green underline indicates a warning.</span></span> <span data-ttu-id="fd8c4-159">紅色底線表示您必須修正的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="fd8c4-160">將滑鼠指標停留`myStr`若要查看會告訴您有關警告的工具提示。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="fd8c4-161">此外，您的滑鼠指標停留紅色底線，以查看錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="fd8c4-162">下圖顯示以底線程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="fd8c4-163">![在 [設計] 檢視中的歡迎文字](code-editing-in-web-forms-pages/_static/image5.png "設計 檢視中的歡迎文字")</span><span class="sxs-lookup"><span data-stu-id="fd8c4-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="fd8c4-164">必須修正錯誤，加上分號`;`一行的結尾。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="fd8c4-165">警告只會通知您，您還沒有使用`myStr`尚未變數。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="fd8c4-166">檢視您目前的程式碼格式化選取的 Visual Studio 中的設定**工具** - &gt; **選項** - &gt; **字型和色彩**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="fd8c4-167">重構和重新命名</span><span class="sxs-lookup"><span data-stu-id="fd8c4-167">Refactoring and Renaming</span></span>

<span data-ttu-id="fd8c4-168">重構是一種軟體的方法，包括重新建構您的程式碼更容易了解和維護，同時保留其功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="fd8c4-169">簡單的範例可能是您從資料庫取得資料的事件處理常式撰寫程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="fd8c4-170">當您開發您的頁面時，您會發現您需要從多個不同的處理常式存取資料。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="fd8c4-171">因此，您會藉由建立網頁中的資料存取方法的處理常式中插入方法的呼叫，來重構網頁的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="fd8c4-172">程式碼編輯器包含可協助您執行重構的各種工作的工具。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="fd8c4-173">在本逐步解說中，您將使用兩個重構的方法： 重新命名變數，並擷取方法。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="fd8c4-174">其他重構的選項包括封裝欄位、 升級至方法參數的本機變數，以及管理方法的參數。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="fd8c4-175">這些重構選項的可用性取決於程式碼中的位置。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="fd8c4-176">重構程式碼</span><span class="sxs-lookup"><span data-stu-id="fd8c4-176">Refactoring Code</span></span>

<span data-ttu-id="fd8c4-177">常見的重構案例是建立 (extract) 從另一個成員，例如方法內的程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="fd8c4-178">這可減少原始成員的大小，並可擷取的程式碼可重複使用。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="fd8c4-179">在本逐步解說的這個部分，您會撰寫一些簡單的程式碼，並再從中擷取方法。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="fd8c4-180">重構支援 C# 中，因此您將建立使用 C# 做為程式設計語言的頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="fd8c4-181">若要擷取的 C# 頁面方法</span><span class="sxs-lookup"><span data-stu-id="fd8c4-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="fd8c4-182">若要切換**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="fd8c4-183">在 **工具箱**，從**標準**索引標籤，拖曳[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)拖曳至網頁。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="fd8c4-184">按兩下** 按鈕**控制項來建立的處理常式及其[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件，然後加入下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="fd8c4-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="fd8c4-185">程式碼會建立**ArrayList**物件，載入的值，會使用迴圈，然後再使用另一個迴圈顯示的內容**ArrayList**物件。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="fd8c4-186">按下**CTRL + F5**來執行頁面，然後按一下** 按鈕**藉此確定您會看到下列輸出：</span><span class="sxs-lookup"><span data-stu-id="fd8c4-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="fd8c4-187">返回 程式碼編輯器，，，然後選取 事件處理常式中的 下列幾行。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="fd8c4-188">以滑鼠右鍵按一下選取範圍中，按一下**重構**，然後選擇**擷取方法**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="fd8c4-189">**擷取方法** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="fd8c4-190">在 **新的方法名稱**方塊中，輸入**DisplayArray**，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="fd8c4-191">程式碼編輯器中建立名為的新方法`DisplayArray`，並將放在新的方法呼叫**按一下**迴圈的最初的處理常式。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="fd8c4-192">按下**CTRL + F5**頁面上再次執行，然後按一下** 按鈕**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="fd8c4-193">網頁的運作與以前一樣。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-193">The page functions the same as it did before.</span></span> <span data-ttu-id="fd8c4-194">`DisplayArray`方法現在可以從任何地方的呼叫，頁面類別中。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="fd8c4-195">重新命名變數</span><span class="sxs-lookup"><span data-stu-id="fd8c4-195">Renaming Variables</span></span>

<span data-ttu-id="fd8c4-196">當您使用變數，以及物件時，您可能想要將其重新命名之後在程式碼中參考。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="fd8c4-197">不過，重新命名的變數和物件可能會導致中斷如果您未重新命名其中一個參考的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="fd8c4-198">因此，您可以使用重構執行重新命名。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="fd8c4-199">若要使用的變數重新命名的重構</span><span class="sxs-lookup"><span data-stu-id="fd8c4-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="fd8c4-200">在 **按一下**事件處理常式中，找到下列這行：</span><span class="sxs-lookup"><span data-stu-id="fd8c4-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="fd8c4-201">變數名稱上按一下滑鼠右鍵`alist`，選擇**重構**，然後選擇**重新命名**。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="fd8c4-202">**重新命名** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="fd8c4-203">在 **新的名稱**方塊中，輸入**ArrayList1** ，並確定**預覽參考變更**在選取核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="fd8c4-204">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="fd8c4-204">Then click **OK**.</span></span>

    <span data-ttu-id="fd8c4-205">**預覽變更**對話方塊隨即出現，並顯示包含您要重新命名的變數的所有參考的樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="fd8c4-206">按一下 [**套用**以關閉**預覽變更**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="fd8c4-207">特別是指您所選取的執行個體的變數會重新命名。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="fd8c4-208">不過請注意，變數`alist`下行中不會重新命名。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="fd8c4-209">變數`alist`此行中不會重新命名，因為它不代表相同的值作為變數`alist`已重新命名。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="fd8c4-210">變數`alist`在`DisplayArray`宣告是針對該方法的區域變數。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="fd8c4-211">這說明使用重構重新命名變數不同於直接執行編輯器; 中的 尋找和取代動作了解其所使用的變數語意的重構重新命名變數。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="fd8c4-212">插入程式碼片段</span><span class="sxs-lookup"><span data-stu-id="fd8c4-212">Inserting Snippets</span></span>

<span data-ttu-id="fd8c4-213">因為有許多 Web Form 開發人員經常需要執行的程式碼撰寫工作，所以程式碼編輯器中提供程式碼片段或預先撰寫的程式碼區塊的程式的庫。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="fd8c4-214">您可以將這些程式碼片段插入您的頁面。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="fd8c4-215">使用 Visual Studio 中的每種語言有插入程式碼片段的方式有些微的差異。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="fd8c4-216">插入程式碼片段的相關資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/library/18yz4be4.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="fd8c4-217">如需插入程式碼片段中 Visual C# 中的資訊，請參閱[Visual C# 程式碼片段](https://msdn.microsoft.com/library/z41h7fat.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd8c4-218">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fd8c4-218">Next Steps</span></span>

<span data-ttu-id="fd8c4-219">本逐步解說已說明 Visual Studio 2010 程式碼編輯器的基本功能的修正程式碼中的錯誤、 重構程式碼、 重新命名變數，和您的程式碼中插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="fd8c4-220">在編輯器中的其他功能可讓應用程式開發快速又簡單。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="fd8c4-221">例如，您可能要：</span><span class="sxs-lookup"><span data-stu-id="fd8c4-221">For example, you might want to:</span></span>

- <span data-ttu-id="fd8c4-222">深入了解 IntelliSense，例如修改 IntelliSense 選項、 管理程式碼片段，以及搜尋線上程式碼片段的功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="fd8c4-223">如需詳細資訊，請參閱[使用 IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="fd8c4-224">了解如何建立您自己的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="fd8c4-225">如需詳細資訊，請參閱[建立及使用 IntelliSense 程式碼片段](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="fd8c4-226">深入了解 IntelliSense 程式碼片段，例如自訂程式碼片段和疑難排解的 Visual Basic 特定功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="fd8c4-227">如需詳細資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="fd8c4-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="fd8c4-228">深入了解 C#-的 IntelliSense、 重構和程式碼片段等的特定功能。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="fd8c4-229">如需詳細資訊，請參閱 < [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fd8c4-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
