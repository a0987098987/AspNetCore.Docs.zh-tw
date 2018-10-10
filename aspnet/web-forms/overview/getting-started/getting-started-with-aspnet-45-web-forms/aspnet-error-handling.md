---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET 錯誤處理 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: af5e5a9c8d211b07b57aa50238b02cabe249aef8
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911836"
---
<a name="aspnet-error-handling"></a><span data-ttu-id="fe68d-103">ASP.NET 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="fe68d-103">ASP.NET Error Handling</span></span>
====================
<span data-ttu-id="fe68d-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="fe68d-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="fe68d-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="fe68d-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="fe68d-106">本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="fe68d-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="fe68d-107">Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="fe68d-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="fe68d-108">在本教學課程中，您將修改 Wingtip Toys 範例應用程式，以包含錯誤處理和錯誤記錄。</span><span class="sxs-lookup"><span data-stu-id="fe68d-108">In this tutorial, you will modify the Wingtip Toys sample application to include error handling and error logging.</span></span> <span data-ttu-id="fe68d-109">錯誤處理可讓應用程式依正常程序處理錯誤，並據以顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="fe68d-109">Error handling will allow the application to gracefully handle errors and display error messages accordingly.</span></span> <span data-ttu-id="fe68d-110">錯誤記錄可讓您找出並修正所發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-110">Error logging will allow you to find and fix errors that have occurred.</span></span> <span data-ttu-id="fe68d-111">此教學課程先前的教學課程 」 URL 路由 」，並且是 Wingtip Toys 教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="fe68d-111">This tutorial builds on the previous tutorial "URL Routing" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="fe68d-112">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="fe68d-112">What you'll learn:</span></span>

- <span data-ttu-id="fe68d-113">如何新增全域錯誤處理應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="fe68d-113">How to add global error handling to the application's configuration.</span></span>
- <span data-ttu-id="fe68d-114">如何新增錯誤處理應用程式、 頁面和程式碼層級。</span><span class="sxs-lookup"><span data-stu-id="fe68d-114">How to add error handling at the application, page, and code levels.</span></span>
- <span data-ttu-id="fe68d-115">如何記錄錯誤，以便稍後進行檢閱。</span><span class="sxs-lookup"><span data-stu-id="fe68d-115">How to log errors for later review.</span></span>
- <span data-ttu-id="fe68d-116">如何顯示錯誤訊息，不會危及安全性。</span><span class="sxs-lookup"><span data-stu-id="fe68d-116">How to display error messages that do not compromise security.</span></span>
- <span data-ttu-id="fe68d-117">如何實作錯誤記錄模組和處理常式 (ELMAH) 的錯誤記錄。</span><span class="sxs-lookup"><span data-stu-id="fe68d-117">How to implement Error Logging Modules and Handlers (ELMAH) error logging.</span></span>

## <a name="overview"></a><span data-ttu-id="fe68d-118">總覽</span><span class="sxs-lookup"><span data-stu-id="fe68d-118">Overview</span></span>

<span data-ttu-id="fe68d-119">ASP.NET 應用程式必須能夠處理以一致的方式執行期間發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-119">ASP.NET applications must be able to handle errors that occur during execution in a consistent manner.</span></span> <span data-ttu-id="fe68d-120">ASP.NET 會使用 common language runtime (CLR)，可提供這種統一的方式通知應用程式的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-120">ASP.NET uses the common language runtime (CLR), which provides a way of notifying applications of errors in a uniform way.</span></span> <span data-ttu-id="fe68d-121">發生錯誤時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-121">When an error occurs, an exception is thrown.</span></span> <span data-ttu-id="fe68d-122">例外狀況是錯誤、 條件或應用程式遇到非預期的行為。</span><span class="sxs-lookup"><span data-stu-id="fe68d-122">An exception is any error, condition, or unexpected behavior that an application encounters.</span></span>

<span data-ttu-id="fe68d-123">在 .NET Framework 中，例外狀況是從 `System.Exception` 類別繼承的物件。</span><span class="sxs-lookup"><span data-stu-id="fe68d-123">In the .NET Framework, an exception is an object that inherits from the `System.Exception` class.</span></span> <span data-ttu-id="fe68d-124">從發生問題的程式碼區域擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-124">An exception is thrown from an area of code where a problem has occurred.</span></span> <span data-ttu-id="fe68d-125">在呼叫堆疊中向上傳遞的例外狀況，其中應用程式會提供處理例外狀況的程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="fe68d-125">The exception is passed up the call stack to a place where the application provides code to handle the exception.</span></span> <span data-ttu-id="fe68d-126">如果應用程式未處理例外狀況，會強制瀏覽器顯示錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-126">If the application does not handle the exception, the browser is forced to display the error details.</span></span>

<span data-ttu-id="fe68d-127">最佳做法是處理中的程式碼層級中的錯誤`Try` / `Catch` / `Finally`內您的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-127">As a best practice, handle errors in at the code level in `Try`/`Catch`/`Finally` blocks within your code.</span></span> <span data-ttu-id="fe68d-128">嘗試將這些區塊，讓使用者可以修正其發生的內容中的問題。</span><span class="sxs-lookup"><span data-stu-id="fe68d-128">Try to place these blocks so that the user can correct problems in the context in which they occur.</span></span> <span data-ttu-id="fe68d-129">如果錯誤處理區塊太遠而從發生錯誤，它變得更難使用者提供他們需要修正此問題的資訊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-129">If the error handling blocks are too far away from where the error occurred, it becomes more difficult to provide users with the information they need to fix the problem.</span></span>

### <a name="exception-class"></a><span data-ttu-id="fe68d-130">例外狀況類別</span><span class="sxs-lookup"><span data-stu-id="fe68d-130">Exception Class</span></span>

<span data-ttu-id="fe68d-131">例外狀況類別是例外狀況從中繼承的基底類別。</span><span class="sxs-lookup"><span data-stu-id="fe68d-131">The Exception class is the base class from which exceptions inherit.</span></span> <span data-ttu-id="fe68d-132">大部分的例外狀況物件是例外狀況類別的一些衍生類別的執行個體，例如`SystemException`類別，`IndexOutOfRangeException`類別，或`ArgumentNullException`類別。</span><span class="sxs-lookup"><span data-stu-id="fe68d-132">Most exception objects are instances of some derived class of the Exception class, such as the `SystemException` class, the `IndexOutOfRangeException` class, or the `ArgumentNullException` class.</span></span> <span data-ttu-id="fe68d-133">此例外狀況類別的屬性，例如`StackTrace`屬性，`InnerException`屬性，而`Message`屬性，提供有關發生之錯誤的特定資訊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-133">The Exception class has properties, such as the `StackTrace` property, the `InnerException` property, and the `Message` property, that provide specific information about the error that has occurred.</span></span>

### <a name="exception-inheritance-hierarchy"></a><span data-ttu-id="fe68d-134">例外狀況的繼承階層架構</span><span class="sxs-lookup"><span data-stu-id="fe68d-134">Exception Inheritance Hierarchy</span></span>

<span data-ttu-id="fe68d-135">執行階段具有例外狀況衍生自基底集合`SystemException`例外狀況發生時，會擲回執行階段的類別。</span><span class="sxs-lookup"><span data-stu-id="fe68d-135">The runtime has a base set of exceptions deriving from the `SystemException` class that the runtime throws when an exception is encountered.</span></span> <span data-ttu-id="fe68d-136">大部分的類別，繼承自例外狀況類別，例如`IndexOutOfRangeException`類別和`ArgumentNullException`類別中，不會實作其他成員。</span><span class="sxs-lookup"><span data-stu-id="fe68d-136">Most of the classes that inherit from the Exception class, such as the `IndexOutOfRangeException` class and the `ArgumentNullException` class, do not implement additional members.</span></span> <span data-ttu-id="fe68d-137">因此，可以找到例外狀況最重要的資訊，階層中的例外狀況、 例外狀況名稱和例外狀況中所包含的資訊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-137">Therefore, the most important information for an exception can be found in the hierarchy of exceptions, the exception name, and the information contained in the exception.</span></span>

### <a name="exception-handling-hierarchy"></a><span data-ttu-id="fe68d-138">例外狀況處理階層架構</span><span class="sxs-lookup"><span data-stu-id="fe68d-138">Exception Handling Hierarchy</span></span>

<span data-ttu-id="fe68d-139">ASP.NET Web Forms 應用程式，可以根據特定的處理階層來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-139">In an ASP.NET Web Forms application, exceptions can be handled based on a specific handling hierarchy.</span></span> <span data-ttu-id="fe68d-140">在下列層級，可以處理例外狀況：</span><span class="sxs-lookup"><span data-stu-id="fe68d-140">An exception can be handled at the following levels:</span></span>

- <span data-ttu-id="fe68d-141">應用程式層級</span><span class="sxs-lookup"><span data-stu-id="fe68d-141">Application level</span></span>
- <span data-ttu-id="fe68d-142">頁面層級</span><span class="sxs-lookup"><span data-stu-id="fe68d-142">Page level</span></span>
- <span data-ttu-id="fe68d-143">程式碼層級</span><span class="sxs-lookup"><span data-stu-id="fe68d-143">Code level</span></span>

<span data-ttu-id="fe68d-144">當應用程式處理的例外狀況時，繼承自例外狀況類別的例外狀況的其他資訊可以通常可以擷取和顯示給使用者。</span><span class="sxs-lookup"><span data-stu-id="fe68d-144">When an application handles exceptions, additional information about the exception that is inherited from the Exception class can often be retrieved and displayed to the user.</span></span> <span data-ttu-id="fe68d-145">除了應用程式、 頁面和程式碼層級，您也可以處理例外狀況的 HTTP 模組層級和使用 IIS 的自訂處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-145">In addition to application, page, and code level, you can also handle exceptions at the HTTP module level and by using an IIS custom handler.</span></span>

### <a name="application-level-error-handling"></a><span data-ttu-id="fe68d-146">應用程式層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="fe68d-146">Application Level Error Handling</span></span>

<span data-ttu-id="fe68d-147">您可以透過修改您的應用程式組態或是加入，處理應用程式層級的預設錯誤`Application_Error`中的處理常式*Global.asax*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-147">You can handle default errors at the application level either by modifying your application's configuration or by adding an `Application_Error` handler in the *Global.asax* file of your application.</span></span>

<span data-ttu-id="fe68d-148">您可以藉由新增處理預設錯誤和 HTTP 錯誤`customErrors`一節*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-148">You can handle default errors and HTTP errors by adding a `customErrors` section to the *Web.config* file.</span></span> <span data-ttu-id="fe68d-149">`customErrors`區段可讓您指定的使用者會重新導向至發生錯誤時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-149">The `customErrors` section allows you to specify a default page that users will be redirected to when an error occurs.</span></span> <span data-ttu-id="fe68d-150">它也可讓您指定個別頁面特定狀態的程式碼錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-150">It also allows you to specify individual pages for specific status code errors.</span></span>

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

<span data-ttu-id="fe68d-151">不幸的是，當您將使用者重新導向至其他頁面使用的組態，您並沒有發生之錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-151">Unfortunately, when you use the configuration to redirect the user to a different page, you do not have the details of the error that occurred.</span></span>

<span data-ttu-id="fe68d-152">不過，將程式碼加入應用程式中捕捉任何一處發生的錯誤`Application_Error`中的處理常式*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-152">However, you can trap errors that occur anywhere in your application by adding code to the `Application_Error` handler in the *Global.asax* file.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a><span data-ttu-id="fe68d-153">頁面層級的錯誤事件處理</span><span class="sxs-lookup"><span data-stu-id="fe68d-153">Page Level Error Event Handling</span></span>

<span data-ttu-id="fe68d-154">頁面層級的處理常式會傳回至頁面的使用者發生錯誤，但因為不會維護控制項的執行個體，將不會再有任何項目頁面上。</span><span class="sxs-lookup"><span data-stu-id="fe68d-154">A page-level handler returns the user to the page where the error occurred, but because instances of controls are not maintained, there will no longer be anything on the page.</span></span> <span data-ttu-id="fe68d-155">若要向應用程式的使用者提供錯誤詳細資料，您必須特別寫入錯誤詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-155">To provide the error details to the user of the application, you must specifically write the error details to the page.</span></span>

<span data-ttu-id="fe68d-156">記錄未處理的錯誤，或將使用者帶到可顯示有用資訊的頁面，您通常會使用頁面層級的錯誤處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-156">You would typically use a page-level error handler to log unhandled errors or to take the user to a page that can display helpful information.</span></span>

<span data-ttu-id="fe68d-157">此程式碼範例顯示 ASP.NET 網頁中的錯誤事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-157">This code example shows a handler for the Error event in an ASP.NET Web page.</span></span> <span data-ttu-id="fe68d-158">此處理常式會攔截所有尚未內處理的例外狀況`try` / `catch`頁面中的區塊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-158">This handler catches all exceptions that are not already handled within `try`/`catch` blocks in the page.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

<span data-ttu-id="fe68d-159">處理錯誤之後，您必須清除它藉由呼叫`ClearError`方法之伺服器物件 (`HttpServerUtility`類別)，否則您會看到先前已發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-159">After you handle an error, you must clear it by calling the `ClearError` method of the Server object (`HttpServerUtility` class), otherwise you will see an error that has previously occurred.</span></span>

### <a name="code-level-error-handling"></a><span data-ttu-id="fe68d-160">程式碼層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="fe68d-160">Code Level Error Handling</span></span>

<span data-ttu-id="fe68d-161">Try / catch 陳述式包含在 try 區塊後面接著一個或多個 catch 子句，指定不同的例外狀況的處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-161">The try-catch statement consists of a try block followed by one or more catch clauses, which specify handlers for different exceptions.</span></span> <span data-ttu-id="fe68d-162">擲回例外狀況時，common language runtime (CLR) 會尋找處理此例外狀況的 catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-162">When an exception is thrown, the common language runtime (CLR) looks for the catch statement that handles this exception.</span></span> <span data-ttu-id="fe68d-163">如果目前執行的方法不會包含 catch 區塊，CLR 會探討呼叫目前的方法，並依此類推項目，在呼叫堆疊的方法。</span><span class="sxs-lookup"><span data-stu-id="fe68d-163">If the currently executing method does not contain a catch block, the CLR looks at the method that called the current method, and so on, up the call stack.</span></span> <span data-ttu-id="fe68d-164">如果找不到任何的 catch 區塊，CLR 就向使用者顯示未處理的例外狀況訊息檔案，並停止程式執行中。</span><span class="sxs-lookup"><span data-stu-id="fe68d-164">If no catch block is found, then the CLR displays an unhandled exception message to the user and stops execution of the program.</span></span>

<span data-ttu-id="fe68d-165">下列程式碼範例顯示使用的常見方式`try` / `catch` / `finally`來處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-165">The following code example shows a common way of using `try`/`catch`/`finally` to handle errors.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

<span data-ttu-id="fe68d-166">在上述程式碼中，try 區塊包含必須加以防範可能的例外狀況的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe68d-166">In the above code, the try block contains the code that needs to be guarded against a possible exception.</span></span> <span data-ttu-id="fe68d-167">區塊會執行直到例外狀況會擲回或區塊已順利完成。</span><span class="sxs-lookup"><span data-stu-id="fe68d-167">The block is executed until either an exception is thrown or the block is completed successfully.</span></span> <span data-ttu-id="fe68d-168">如果有任一`FileNotFoundException`例外狀況或`IOException`發生例外狀況，執行會轉移至不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-168">If either a `FileNotFoundException` exception or an `IOException` exception occurs, the execution is transferred to a different page.</span></span> <span data-ttu-id="fe68d-169">然後，在包含的程式碼會執行 finally 區塊，是否發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-169">Then, the code contained in the finally block is executed, whether an error occurred or not.</span></span>

## <a name="adding-error-logging-support"></a><span data-ttu-id="fe68d-170">新增錯誤記錄的支援</span><span class="sxs-lookup"><span data-stu-id="fe68d-170">Adding Error Logging Support</span></span>

<span data-ttu-id="fe68d-171">然後再加入錯誤處理 「 Wingtip Toys 範例應用程式，您會新增錯誤記錄支援加上`ExceptionUtility`類別，即可*邏輯*資料夾。</span><span class="sxs-lookup"><span data-stu-id="fe68d-171">Before adding error handling to the Wingtip Toys sample application, you will add error logging support by adding an `ExceptionUtility` class to the *Logic* folder.</span></span> <span data-ttu-id="fe68d-172">透過這種方式，每次應用程式處理錯誤，錯誤詳細資料會加入錯誤記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fe68d-172">By doing this, each time the application handles an error, the error details will be added to the error log file.</span></span>

1. <span data-ttu-id="fe68d-173">以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-173">Right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="fe68d-174">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-174">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="fe68d-175">選取  **Visual C#**  - &gt; **程式碼**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="fe68d-175">Select the **Visual C#** -&gt; **Code** templates group on the left.</span></span> <span data-ttu-id="fe68d-176">然後，選取**類別**從中間清單並將它命名**ExceptionUtility.cs**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-176">Then, select **Class**from the middle list and name it **ExceptionUtility.cs**.</span></span>
3. <span data-ttu-id="fe68d-177">選擇 [新增]。</span><span class="sxs-lookup"><span data-stu-id="fe68d-177">Choose **Add**.</span></span> <span data-ttu-id="fe68d-178">會顯示新的類別檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-178">The new class file is displayed.</span></span>
4. <span data-ttu-id="fe68d-179">將現有的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe68d-179">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

<span data-ttu-id="fe68d-180">當發生例外狀況時，例外狀況可以寫入至例外狀況記錄檔呼叫`LogException`方法。</span><span class="sxs-lookup"><span data-stu-id="fe68d-180">When an exception occurs, the exception can be written to an exception log file by calling the `LogException` method.</span></span> <span data-ttu-id="fe68d-181">這個方法會採用兩個參數、 例外狀況物件和字串，包含有關例外狀況來源的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-181">This method takes two parameters, the exception object and a string containing details about the source of the exception.</span></span> <span data-ttu-id="fe68d-182">例外狀況記錄檔會寫入*ErrorLog.txt*中的檔案*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="fe68d-182">The exception log is written to the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>

### <a name="adding-an-error-page"></a><span data-ttu-id="fe68d-183">加入錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="fe68d-183">Adding an Error Page</span></span>

<span data-ttu-id="fe68d-184">在 Wingtip Toys 範例應用程式中，一頁將用於顯示的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-184">In the Wingtip Toys sample application, one page will be used to display errors.</span></span> <span data-ttu-id="fe68d-185">[錯誤] 頁面可向網站的使用者顯示安全的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="fe68d-185">The error page is designed to show a secure error message to users of the site.</span></span> <span data-ttu-id="fe68d-186">不過，如果使用者是開發人員提出 HTTP 要求正在電腦上本機服務的程式碼位於何處，其他錯誤詳細資料會顯示在 [錯誤] 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-186">However, if the user is a developer making an HTTP request that is being served locally on the machine where the code lives, additional error details will be displayed on the error page.</span></span>

1. <span data-ttu-id="fe68d-187">以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管**，然後選取**新增** - &gt; **新項目**.</span><span class="sxs-lookup"><span data-stu-id="fe68d-187">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="fe68d-188">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-188">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="fe68d-189">選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="fe68d-189">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="fe68d-190">從清單的中間，選取**使用主版頁面的 Web Form**，並將它命名**ErrorPage.aspx**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-190">From the middle list, select **Web Form with Master Page**,and name it **ErrorPage.aspx**.</span></span>
3. <span data-ttu-id="fe68d-191">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="fe68d-191">Click **Add**.</span></span>
4. <span data-ttu-id="fe68d-192">選取  *Site.Master*主版頁面中，為檔案，然後再選擇**確定**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-192">Select the *Site.Master* file as the master page, and then choose **OK**.</span></span>
5. <span data-ttu-id="fe68d-193">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="fe68d-193">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. <span data-ttu-id="fe68d-194">取代現有的程式碼的程式碼後置 (*ErrorPage.aspx.cs*)，使其出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="fe68d-194">Replace the existing code of the code-behind (*ErrorPage.aspx.cs*) so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

<span data-ttu-id="fe68d-195">顯示錯誤頁面時，請`Page_Load`事件處理常式會執行。</span><span class="sxs-lookup"><span data-stu-id="fe68d-195">When the error page is displayed, the `Page_Load` event handler is executed.</span></span> <span data-ttu-id="fe68d-196">在 `Page_Load`處理常式中，決定先處理此錯誤的位置。</span><span class="sxs-lookup"><span data-stu-id="fe68d-196">In the `Page_Load` handler, the location of where the error was first handled is determined.</span></span> <span data-ttu-id="fe68d-197">然後，最後發生的錯誤由呼叫`GetLastError`伺服器物件的方法。</span><span class="sxs-lookup"><span data-stu-id="fe68d-197">Then, the last error that occurred is determined by call the `GetLastError` method of the Server object.</span></span> <span data-ttu-id="fe68d-198">如果例外狀況不再存在，則會建立一般的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-198">If the exception no longer exists, a generic exception is created.</span></span> <span data-ttu-id="fe68d-199">然後，如果已在本機進行 HTTP 要求，就會顯示所有的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-199">Then, if the HTTP request was made locally, all error details are shown.</span></span> <span data-ttu-id="fe68d-200">在此情況下，只有本機電腦執行的 web 應用程式會看到這些錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-200">In this case, only the local machine running the web application will see these error details.</span></span> <span data-ttu-id="fe68d-201">顯示錯誤訊息後，錯誤會加入至記錄檔和錯誤都會從伺服器。</span><span class="sxs-lookup"><span data-stu-id="fe68d-201">After the error information has been displayed, the error is added to the log file and the error is cleared from the server.</span></span>

### <a name="displaying-unhandled-error-messages-for-the-application"></a><span data-ttu-id="fe68d-202">顯示應用程式的未處理的錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="fe68d-202">Displaying Unhandled Error Messages for the Application</span></span>

<span data-ttu-id="fe68d-203">藉由新增`customErrors`一節*Web.config*檔案中，您可以快速地處理簡單整個應用程式就會發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-203">By adding a `customErrors` section to the *Web.config* file, you can quickly handle simple errors that occur throughout the application.</span></span> <span data-ttu-id="fe68d-204">您也可以指定如何處理錯誤狀態碼值，例如 404-找不到檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-204">You can also specify how to handle errors based on their status code value, such as 404 - File not found.</span></span>

#### <a name="update-the-configuration"></a><span data-ttu-id="fe68d-205">請更新組態</span><span class="sxs-lookup"><span data-stu-id="fe68d-205">Update the Configuration</span></span>

<span data-ttu-id="fe68d-206">更新組態，藉由新增`customErrors`一節*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-206">Update the configuration by adding a `customErrors` section to the *Web.config* file.</span></span>

1. <span data-ttu-id="fe68d-207">在 **方案總管**，尋找並開啟*Web.config* Wingtip Toys 範例應用程式根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-207">In **Solution Explorer**, find and open the *Web.config* file at the root of the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="fe68d-208">新增`customErrors`一節*Web.config*檔案內`<system.web>`節點，如下所示：</span><span class="sxs-lookup"><span data-stu-id="fe68d-208">Add the `customErrors` section to the *Web.config* file within the `<system.web>` node as follows:</span></span>   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. <span data-ttu-id="fe68d-209">儲存*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-209">Save the *Web.config* file.</span></span>

<span data-ttu-id="fe68d-210">`customErrors`區段指定的模式，它會設定為 [開啟]。</span><span class="sxs-lookup"><span data-stu-id="fe68d-210">The `customErrors` section specifies the mode, which is set to "On".</span></span> <span data-ttu-id="fe68d-211">它也會指定`defaultRedirect`，這會告知應用程式發生錯誤時，瀏覽至頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-211">It also specifies the `defaultRedirect`, which tells the application which page to navigate to when an error occurs.</span></span> <span data-ttu-id="fe68d-212">此外，您已新增指定如何處理 404 錯誤，找不到頁面時的特定錯誤項目。</span><span class="sxs-lookup"><span data-stu-id="fe68d-212">In addition, you have added a specific error element that specifies how to handle a 404 error when a page is not found.</span></span> <span data-ttu-id="fe68d-213">稍後在本教學課程中，您將加入其他錯誤處理，將會擷取應用程式層級錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-213">Later in this tutorial, you will add additional error handling that will capture the details of an error at the application level.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="fe68d-214">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fe68d-214">Running the Application</span></span>

<span data-ttu-id="fe68d-215">您可以執行應用程式現在若要查看更新的路由。</span><span class="sxs-lookup"><span data-stu-id="fe68d-215">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="fe68d-216">按下**F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-216">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="fe68d-217">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-217">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="fe68d-218">在瀏覽器中輸入下列 URL (請務必使用**您**連接埠號碼):</span><span class="sxs-lookup"><span data-stu-id="fe68d-218">Enter the following URL into the browser (be sure to use **your** port number):</span></span>  
    `https://localhost:44300/NoPage.aspx`
3. <span data-ttu-id="fe68d-219">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="fe68d-219">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理： 找不到頁面時發生錯誤](aspnet-error-handling/_static/image1.png)

<span data-ttu-id="fe68d-221">當您要求*NoPage.aspx*頁面上，不存在，[錯誤] 頁面會顯示簡單的錯誤訊息和詳細的錯誤資訊如果可用的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-221">When you request the *NoPage.aspx* page, which does not exist, the error page will show the simple error message and the detailed error information if additional details are available.</span></span> <span data-ttu-id="fe68d-222">不過，如果使用者從遠端位置要求不存在的頁面，[錯誤] 頁面會只顯示錯誤訊息以紅色。</span><span class="sxs-lookup"><span data-stu-id="fe68d-222">However, if the user requested a non-existent page from a remote location, the error page would only show the error message in red.</span></span>

### <a name="including-an-exception-for-testing-purposes"></a><span data-ttu-id="fe68d-223">基於測試目的，包括例外狀況</span><span class="sxs-lookup"><span data-stu-id="fe68d-223">Including an Exception for Testing Purposes</span></span>

<span data-ttu-id="fe68d-224">若要確認您的應用程式的運作方式錯誤時，就會發生，在 ASP.NET 中刻意造成錯誤狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-224">To verify how your application will function when an error occurs, you can deliberately create error conditions in ASP.NET.</span></span> <span data-ttu-id="fe68d-225">在 Wingtip Toys 範例應用程式中，您將會擲回測試例外狀況在預設頁面載入時要看看結果如何。</span><span class="sxs-lookup"><span data-stu-id="fe68d-225">In the Wingtip Toys sample application, you will throw a test exception when the default page loads to see what happens.</span></span>

1. <span data-ttu-id="fe68d-226">開啟的程式碼後置*Default.aspx* Visual Studio 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-226">Open the code-behind of the *Default.aspx* page in Visual Studio.</span></span>   
   <span data-ttu-id="fe68d-227">*Default.aspx.cs*將會顯示程式碼後置頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-227">The *Default.aspx.cs* code-behind page will be displayed.</span></span>
2. <span data-ttu-id="fe68d-228">在 `Page_Load`處理常式，加入程式碼，以便處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="fe68d-228">In the `Page_Load` handler, add code so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

<span data-ttu-id="fe68d-229">可以建立各種不同例外狀況類型。</span><span class="sxs-lookup"><span data-stu-id="fe68d-229">It is possible to create various different types of exceptions.</span></span> <span data-ttu-id="fe68d-230">在上述程式碼中，您會建立`InvalidOperationException`時*Default.aspx*載入頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-230">In the above code, you are creating an `InvalidOperationException` when the *Default.aspx* page is loaded.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="fe68d-231">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fe68d-231">Running the Application</span></span>

<span data-ttu-id="fe68d-232">您可以執行的應用程式，以查看應用程式如何處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-232">You can run the application to see how the application handles the exception.</span></span>

1. <span data-ttu-id="fe68d-233">按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-233">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="fe68d-234">應用程式會擲回 InvalidOperationException。</span><span class="sxs-lookup"><span data-stu-id="fe68d-234">The application throws the InvalidOperationException.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="fe68d-235">您必須按下**CTRL + F5**顯示網頁，而不需要分成多個程式碼以在 Visual Studio 中檢視錯誤的來源。</span><span class="sxs-lookup"><span data-stu-id="fe68d-235">You must press **CTRL+F5** to display the page without breaking into the code to view the source of the error in Visual Studio.</span></span>
2. <span data-ttu-id="fe68d-236">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="fe68d-236">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理-錯誤頁面](aspnet-error-handling/_static/image2.png)

<span data-ttu-id="fe68d-238">如您所見錯誤詳細資料，例外狀況已受困於連結`customError`一節*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-238">As you can see in the error details, the exception was trapped by the `customError` section in the *Web.config* file.</span></span>

### <a name="adding-application-level-error-handling"></a><span data-ttu-id="fe68d-239">加入應用程式層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="fe68d-239">Adding Application-Level Error Handling</span></span>

<span data-ttu-id="fe68d-240">而不是例外狀況使用的設陷`customErrors`一節*Web.config*檔案中，在您取得例外狀況的極少資訊時，您可以捕捉此錯誤，應用程式層級，並擷取錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-240">Rather than trap the exception using the `customErrors` section in the *Web.config* file, where you gain little information about the exception, you can trap the error at the application level and retrieve error details.</span></span>

1. <span data-ttu-id="fe68d-241">在 **方案總管**，尋找並開啟*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-241">In **Solution Explorer**, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="fe68d-242">新增**應用程式\_錯誤**處理常式，以便它看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="fe68d-242">Add an **Application\_Error** handler so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

<span data-ttu-id="fe68d-243">在 應用程式時，發生錯誤時`Application_Error`呼叫處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-243">When an error occurs in the application, the `Application_Error` handler is called.</span></span> <span data-ttu-id="fe68d-244">在這個處理常式中，上次的例外狀況是擷取和檢閱。</span><span class="sxs-lookup"><span data-stu-id="fe68d-244">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="fe68d-245">如果未處理的例外狀況和例外狀況包含內部例外狀況詳細資料 (也就是`InnerException`不是 null)，應用程式將執行轉移至 [錯誤] 頁面會顯示例外狀況詳細資料的位置。</span><span class="sxs-lookup"><span data-stu-id="fe68d-245">If the exception was unhandled and the exception contains inner-exception details (that is, `InnerException` is not null), the application transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="fe68d-246">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fe68d-246">Running the Application</span></span>

<span data-ttu-id="fe68d-247">您可以執行的應用程式，以查看所處理的例外狀況，應用程式層級提供的其他錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fe68d-247">You can run the application to see the additional error details provided by handling the exception at the application level.</span></span>

1. <span data-ttu-id="fe68d-248">按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-248">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="fe68d-249">應用程式會擲回`InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="fe68d-249">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="fe68d-250">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="fe68d-250">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理： 應用程式層級錯誤](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a><span data-ttu-id="fe68d-252">加入頁面層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="fe68d-252">Adding Page-Level Error Handling</span></span>

<span data-ttu-id="fe68d-253">您可以加入頁面層級的錯誤處理頁面使用 新增`ErrorPage`屬性設定為`@Page`指示詞的頁面上，或藉由新增`Page_Error`頁面的程式碼後置的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-253">You can add page-level error handling to a page either by using adding an `ErrorPage` attribute to the `@Page` directive of the page, or by adding a `Page_Error` event handler to the code-behind of a page.</span></span> <span data-ttu-id="fe68d-254">在本節中，您將新增`Page_Error`就會傳送至執行的事件處理常式*ErrorPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-254">In this section, you will add a `Page_Error` event handler that will transfer execution to the *ErrorPage.aspx* page.</span></span>

1. <span data-ttu-id="fe68d-255">在 **方案總管**，尋找並開啟*Default.aspx.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-255">In **Solution Explorer**, find and open the *Default.aspx.cs* file.</span></span>
2. <span data-ttu-id="fe68d-256">新增`Page_Error`處理常式，讓程式碼後置會顯示為追蹤：</span><span class="sxs-lookup"><span data-stu-id="fe68d-256">Add a `Page_Error` handler so that the code-behind appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

<span data-ttu-id="fe68d-257">在頁面上，發生錯誤時`Page_Error`會呼叫事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-257">When an error occurs on the page, the `Page_Error` event handler is called.</span></span> <span data-ttu-id="fe68d-258">在這個處理常式中，上次的例外狀況是擷取和檢閱。</span><span class="sxs-lookup"><span data-stu-id="fe68d-258">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="fe68d-259">如果`InvalidOperationException`發生時，`Page_Error`事件處理常式將執行轉移至 [錯誤] 頁面會顯示例外狀況詳細資料的位置。</span><span class="sxs-lookup"><span data-stu-id="fe68d-259">If an `InvalidOperationException` occurs, the `Page_Error` event handler transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="fe68d-260">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fe68d-260">Running the Application</span></span>

<span data-ttu-id="fe68d-261">您可以執行應用程式現在若要查看更新的路由。</span><span class="sxs-lookup"><span data-stu-id="fe68d-261">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="fe68d-262">按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-262">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="fe68d-263">應用程式會擲回`InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="fe68d-263">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="fe68d-264">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="fe68d-264">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理-頁面層級錯誤](aspnet-error-handling/_static/image4.png)
3. <span data-ttu-id="fe68d-266">關閉瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="fe68d-266">Close your browser window.</span></span>

### <a name="removing-the-exception-used-for-testing"></a><span data-ttu-id="fe68d-267">移除用於測試的例外狀況</span><span class="sxs-lookup"><span data-stu-id="fe68d-267">Removing the Exception Used for Testing</span></span>

<span data-ttu-id="fe68d-268">若要讓 Wingtip Toys 範例應用程式，而不擲回的例外狀況，您稍早在本教學課程中新增函式，移除 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-268">To allow the Wingtip Toys sample application to function without throwing the exception you added earlier in this tutorial, remove the exception.</span></span>

1. <span data-ttu-id="fe68d-269">開啟的程式碼後置*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="fe68d-269">Open the code-behind of the *Default.aspx* page.</span></span>
2. <span data-ttu-id="fe68d-270">在 `Page_Load`處理常式中，移除的程式碼，則會擲回例外狀況，以便處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="fe68d-270">In the `Page_Load` handler, remove the code that throws the exception so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a><span data-ttu-id="fe68d-271">加入程式碼層級的錯誤記錄</span><span class="sxs-lookup"><span data-stu-id="fe68d-271">Adding Code-Level Error Logging</span></span>

<span data-ttu-id="fe68d-272">如稍早在本教學課程中所述，您可以加入 try/catch 陳述式，嘗試執行的程式碼區段，並處理所發生的第一個錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-272">As mentioned earlier in this tutorial, you can add try/catch statements to attempt to run a section of code and handle the first error that occurs.</span></span> <span data-ttu-id="fe68d-273">在此範例中，您將只寫入錯誤詳細資料的錯誤記錄檔，以便您可以稍後檢閱錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-273">In this example, you will only write the error details to the error log file so that the error can be reviewed later.</span></span>

1. <span data-ttu-id="fe68d-274">在 **方案總管**，請在*邏輯*資料夾中，尋找並開啟*PayPalFunctions.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-274">In **Solution Explorer**, in the *Logic* folder, find and open the *PayPalFunctions.cs* file.</span></span>
2. <span data-ttu-id="fe68d-275">更新`HttpCall`方法讓，程式碼會顯示為如下所示：</span><span class="sxs-lookup"><span data-stu-id="fe68d-275">Update the `HttpCall` method so that the code appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

<span data-ttu-id="fe68d-276">上述程式碼會呼叫`LogException`方法中包含`ExceptionUtility`類別。</span><span class="sxs-lookup"><span data-stu-id="fe68d-276">The above code calls the `LogException` method that is contained in the `ExceptionUtility` class.</span></span> <span data-ttu-id="fe68d-277">您加入*ExceptionUtility.cs*類別檔案，以*邏輯*稍早在本教學課程中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fe68d-277">You added the *ExceptionUtility.cs* class file to the *Logic* folder earlier in this tutorial.</span></span> <span data-ttu-id="fe68d-278">`LogException` 方法會接受兩個參數。</span><span class="sxs-lookup"><span data-stu-id="fe68d-278">The `LogException` method takes two parameters.</span></span> <span data-ttu-id="fe68d-279">第一個參數是例外狀況物件。</span><span class="sxs-lookup"><span data-stu-id="fe68d-279">The first parameter is the exception object.</span></span> <span data-ttu-id="fe68d-280">第二個參數是用來識別錯誤來源的字串。</span><span class="sxs-lookup"><span data-stu-id="fe68d-280">The second parameter is a string used to recognize the source of the error.</span></span>

### <a name="inspecting-the-error-logging-information"></a><span data-ttu-id="fe68d-281">檢查錯誤記錄資訊</span><span class="sxs-lookup"><span data-stu-id="fe68d-281">Inspecting the Error Logging Information</span></span>

<span data-ttu-id="fe68d-282">如先前所述，您可以使用錯誤記錄檔，以判斷應該先修正應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-282">As mentioned previously, you can use the error log to determine which errors in your application should be fixed first.</span></span> <span data-ttu-id="fe68d-283">當然，不會記錄錯誤截取和寫入錯誤記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fe68d-283">Of course, only errors that have been trapped and written to the error log will be recorded.</span></span>

1. <span data-ttu-id="fe68d-284">在 **方案總管**，尋找並開啟*ErrorLog.txt*檔案中*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="fe68d-284">In **Solution Explorer**, find and open the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>   
 <span data-ttu-id="fe68d-285">您可能需要選取 」**顯示所有檔案**」 選項或"**重新整理**」 選項，從頂端**方案總管 中**若要查看*ErrorLog.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-285">You may need to select the "**Show All Files**" option or the "**Refresh**" option from the top of **Solution Explorer** to see the *ErrorLog.txt* file.</span></span>
2. <span data-ttu-id="fe68d-286">檢閱顯示在 Visual Studio 中的錯誤記錄檔：</span><span class="sxs-lookup"><span data-stu-id="fe68d-286">Review the error log displayed in Visual Studio:</span></span> 

    ![ASP.NET 錯誤處理-ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a><span data-ttu-id="fe68d-288">安全的錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="fe68d-288">Safe Error Messages</span></span>

<span data-ttu-id="fe68d-289">很**務必注意**，當您的應用程式會顯示錯誤訊息，它不應將惡意使用者可能會發現有幫助攻擊您的應用程式的資訊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-289">It is **important to note** that when your application displays error messages, it should not give away information that a malicious user might find helpful in attacking your application.</span></span> <span data-ttu-id="fe68d-290">例如，如果您的應用程式未能成功嘗試寫入資料庫，它不應該顯示錯誤訊息，其中包含它所使用的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="fe68d-290">For example, if your application unsuccessfully tries to write in to a database, it should not display an error message that includes the user name it is using.</span></span> <span data-ttu-id="fe68d-291">基於這個理由，紅色的一般錯誤訊息會顯示給使用者。</span><span class="sxs-lookup"><span data-stu-id="fe68d-291">For this reason, a generic error message in red is displayed to the user.</span></span> <span data-ttu-id="fe68d-292">所有其他錯誤詳細資料只會顯示在本機電腦上的程式開發人員。</span><span class="sxs-lookup"><span data-stu-id="fe68d-292">All additional error details are only displayed to the developer on the local machine.</span></span>

## <a name="using-elmah"></a><span data-ttu-id="fe68d-293">使用 ELMAH</span><span class="sxs-lookup"><span data-stu-id="fe68d-293">Using ELMAH</span></span>

<span data-ttu-id="fe68d-294">ELMAH （錯誤記錄模組和處理常式） 是您插入您的 ASP.NET 應用程式，以 NuGet 套件形式的錯誤記錄功能。</span><span class="sxs-lookup"><span data-stu-id="fe68d-294">ELMAH (Error Logging Modules and Handlers) is an error logging facility that you plug into your ASP.NET application as a NuGet package.</span></span> <span data-ttu-id="fe68d-295">ELMAH 提供下列功能：</span><span class="sxs-lookup"><span data-stu-id="fe68d-295">ELMAH provides the following capabilities:</span></span>

- <span data-ttu-id="fe68d-296">記錄未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-296">Logging of unhandled exceptions.</span></span>
- <span data-ttu-id="fe68d-297">若要檢視錄製的未處理例外狀況的整個記錄檔的網頁。</span><span class="sxs-lookup"><span data-stu-id="fe68d-297">A web page to view the entire log of recoded unhandled exceptions.</span></span>
- <span data-ttu-id="fe68d-298">若要檢視完整的詳細資料，每個網頁記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-298">A web page to view the full details of each logged exception.</span></span>
- <span data-ttu-id="fe68d-299">它發生時每個錯誤的電子郵件通知。</span><span class="sxs-lookup"><span data-stu-id="fe68d-299">An email notification of each error at the time it occurs.</span></span>
- <span data-ttu-id="fe68d-300">從記錄檔，RSS 摘要的最後 15 個錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-300">An RSS feed of the last 15 errors from the log.</span></span>

<span data-ttu-id="fe68d-301">您可以使用 ELMAH，您必須先安裝它。</span><span class="sxs-lookup"><span data-stu-id="fe68d-301">Before you can work with the ELMAH, you must install it.</span></span> <span data-ttu-id="fe68d-302">這是容易使用*NuGet*套件安裝程式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-302">This is easy using the *NuGet* package installer.</span></span> <span data-ttu-id="fe68d-303">如稍早在本教學課程系列中所述，NuGet 就會是 Visual Studio 擴充功能，可讓您更輕鬆地安裝及更新的開放原始碼程式庫和 Visual Studio 中的工具。</span><span class="sxs-lookup"><span data-stu-id="fe68d-303">As mentioned earlier in this tutorial series, NuGet is a Visual Studio extension that makes it easy to install and update open source libraries and tools in Visual Studio.</span></span>

1. <span data-ttu-id="fe68d-304">在 Visual Studio 中，從**工具**功能表上，選取**NuGet 套件管理員** > **管理方案的 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-304">Within Visual Studio, from the **Tools** menu, select **NuGet Package Manager** > **Manage NuGet Packages for Solution**.</span></span> 

    ![ASP.NET 錯誤處理-管理方案的 NuGet 套件](aspnet-error-handling/_static/image6.png)
2. <span data-ttu-id="fe68d-306">**管理 NuGet 套件**對話方塊隨即出現在 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="fe68d-306">The **Manage NuGet Packages** dialog box is displayed within Visual Studio.</span></span>
3. <span data-ttu-id="fe68d-307">中**管理 NuGet 套件**對話方塊方塊中，展開**線上**左邊，然後選取**nuget.org**。然後，尋找並安裝**ELMAH**從線上可用的套件清單中的封裝。</span><span class="sxs-lookup"><span data-stu-id="fe68d-307">In the **Manage NuGet Packages** dialog box, expand **Online** on the left, and then select **nuget.org**. Then, find and install the **ELMAH** package from the list of available packages online.</span></span> 

    ![ASP.NET 錯誤處理-ELMA NuGet 套件](aspnet-error-handling/_static/image7.png)
4. <span data-ttu-id="fe68d-309">您必須有網際網路連線以下載封裝。</span><span class="sxs-lookup"><span data-stu-id="fe68d-309">You will need to have an internet connection to download the package.</span></span>
5. <span data-ttu-id="fe68d-310">在 **選取專案**對話方塊方塊中，請確定**WingtipToys**選取項目已選取，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-310">In the **Select Projects** dialog box, make sure the **WingtipToys** selection is selected, and then click **OK**.</span></span> 

    ![ASP.NET 錯誤處理： 選取 [專案] 對話方塊](aspnet-error-handling/_static/image8.png)
6. <span data-ttu-id="fe68d-312">按一下 [**關閉**中**管理 NuGet 套件**視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fe68d-312">Click **Close** in the **Manage NuGet Packages** dialog box if needed.</span></span>
7. <span data-ttu-id="fe68d-313">如果 Visual Studio 會要求您重新載入任何開啟的檔案，請選取 「**全部皆是**"。</span><span class="sxs-lookup"><span data-stu-id="fe68d-313">If Visual Studio requests that you reload any open files, select "**Yes to All**".</span></span>
8. <span data-ttu-id="fe68d-314">ELMAH 封裝本身中加入項目*Web.config*您的專案根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="fe68d-314">The ELMAH package adds entries for itself in the *Web.config* file at the root of your project.</span></span> <span data-ttu-id="fe68d-315">如果 Visual Studio 會詢問您是否要重新載入已修改*Web.config*檔案中，按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="fe68d-315">If Visual Studio asks you if you want to reload the modified *Web.config* file, click **Yes**.</span></span>

<span data-ttu-id="fe68d-316">ELMAH 現在就來儲存任何未處理發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-316">ELMAH is now ready to store any unhandled errors that occur.</span></span>

### <a name="viewing-the-elmah-log"></a><span data-ttu-id="fe68d-317">檢視 ELMAH 記錄檔</span><span class="sxs-lookup"><span data-stu-id="fe68d-317">Viewing the ELMAH Log</span></span>

<span data-ttu-id="fe68d-318">檢視 ELMAH 記錄檔很容易，但首先您要建立將會記錄在 ELMAH 記錄未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fe68d-318">Viewing the ELMAH log is easy, but first you will create an unhandled exception that will be recorded in the ELMAH log.</span></span>

1. <span data-ttu-id="fe68d-319">按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe68d-319">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="fe68d-320">若要寫入的 ELMAH 記錄未處理的例外狀況，請瀏覽至下列 URL （使用您的連接埠號碼） 瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="fe68d-320">To write an unhandled exception to the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    <span data-ttu-id="fe68d-321">`https://localhost:44300/NoPage.aspx` [錯誤] 頁面隨即出現。</span><span class="sxs-lookup"><span data-stu-id="fe68d-321">`https://localhost:44300/NoPage.aspx` The error page will be displayed.</span></span>
3. <span data-ttu-id="fe68d-322">若要顯示的 ELMAH 記錄檔，請瀏覽至下列 URL （使用您的連接埠號碼） 瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="fe68d-322">To display the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET 錯誤處理-ELMAH 錯誤記錄檔](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="fe68d-324">總結</span><span class="sxs-lookup"><span data-stu-id="fe68d-324">Summary</span></span>

<span data-ttu-id="fe68d-325">在本教學課程中，您已了解處理的應用程式層級、 頁面層級和程式碼層級的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fe68d-325">In this tutorial, you have learned about handling errors at the application level, the page level, and the code level.</span></span> <span data-ttu-id="fe68d-326">您也學到如何記錄處理和未處理的錯誤，以便稍後進行檢閱。</span><span class="sxs-lookup"><span data-stu-id="fe68d-326">You have also learned how to log handled and unhandled errors for later review.</span></span> <span data-ttu-id="fe68d-327">您已新增 ELMAH 公用程式，以提供例外狀況記錄和您使用 NuGet 的應用程式的通知。</span><span class="sxs-lookup"><span data-stu-id="fe68d-327">You added the ELMAH utility to provide exception logging and notification to your application using NuGet.</span></span> <span data-ttu-id="fe68d-328">此外，您已了解關於安全的錯誤訊息的重要性。</span><span class="sxs-lookup"><span data-stu-id="fe68d-328">Additionally, you have learned about the importance of safe error messages.</span></span>

## <a name="tutorial-series-conclusion"></a><span data-ttu-id="fe68d-329">教學課程系列的結論</span><span class="sxs-lookup"><span data-stu-id="fe68d-329">Tutorial Series Conclusion</span></span>

<span data-ttu-id="fe68d-330">*跟著感謝。我希望這套教學課程可協助您更加熟悉 ASP.NET Web Form。如果您需要提供在 ASP.NET 4.5 和 Visual Studio 2013 中的 Web Form 功能的詳細資訊，請參閱* [ *ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊*](../../../../visual-studio/overview/2013/release-notes.md) *.此外，請務必看看教學課程中所述* \***接下來的步驟 \* \* \* 區段和 defintely 試用* [*免費 Azure 試用*](https://azure.microsoft.com/pricing/free-trial/)*.\*</span><span class="sxs-lookup"><span data-stu-id="fe68d-330">*Thanks for following along. I hope this set of tutorials helped you become more familiar with ASP.NET Web Forms. If you need more information about Web Forms features available in ASP.NET 4.5 and Visual Studio 2013, see* [*ASP.NET and Web Tools for Visual Studio 2013 Release Notes*](../../../../visual-studio/overview/2013/release-notes.md)*. Also, be sure to take a look at the tutorial mentioned in the* \***Next Steps\*\*\*\*section and defintely try out the* [*free Azure trial*](https://azure.microsoft.com/pricing/free-trial/)*.\*</span></span>

![感謝您-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a><span data-ttu-id="fe68d-332">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fe68d-332">Next Steps</span></span>

<span data-ttu-id="fe68d-333">深入了解部署到 Microsoft Azure web 應用程式，請參閱 <<c0> [ 部署至 Azure 網站的安全 ASP.NET Web Forms 應用程式成員資格、 OAuth 和 SQL Database](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)。</span><span class="sxs-lookup"><span data-stu-id="fe68d-333">Learn more about deploying your web application to Microsoft Azure, see [Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Web Site](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).</span></span>

## <a name="free-trial"></a><span data-ttu-id="fe68d-334">免費試用版</span><span class="sxs-lookup"><span data-stu-id="fe68d-334">Free Trial</span></span>

[<span data-ttu-id="fe68d-335">Microsoft Azure-免費試用版</span><span class="sxs-lookup"><span data-stu-id="fe68d-335">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)  
 <span data-ttu-id="fe68d-336">您的網站發佈至 Microsoft Azure 將會節省時間、 維護和費用。</span><span class="sxs-lookup"><span data-stu-id="fe68d-336">Publishing your website to Microsoft Azure will save you time, maintenance and expense.</span></span> <span data-ttu-id="fe68d-337">它是快速的程序，以將您的 web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="fe68d-337">It's a quick process to deploying your web app to Azure.</span></span> <span data-ttu-id="fe68d-338">當您需要維護及監視您的 web 應用程式時，Azure 會提供各種不同的工具和服務。</span><span class="sxs-lookup"><span data-stu-id="fe68d-338">When you need to maintain and monitor your web app, Azure offers a variety of tools and services.</span></span> <span data-ttu-id="fe68d-339">管理資料、 資料傳輸、 身份識別、 備份、 訊息、 媒體和在 Azure 中的效能。</span><span class="sxs-lookup"><span data-stu-id="fe68d-339">Manage data, traffic, identity, backups, messaging, media and performance in Azure.</span></span> <span data-ttu-id="fe68d-340">而且，所有這些都非常符合成本效益的方法中提供。</span><span class="sxs-lookup"><span data-stu-id="fe68d-340">And, all of this is provided in a very cost effective approach.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe68d-341">其他資源</span><span class="sxs-lookup"><span data-stu-id="fe68d-341">Additional Resources</span></span>

<span data-ttu-id="fe68d-342">[使用 ASP.NET 狀況監控記錄錯誤的詳細資料](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span><span class="sxs-lookup"><span data-stu-id="fe68d-342">[Logging Error Details with ASP.NET Health Monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span></span>  
[<span data-ttu-id="fe68d-343">ELMAH</span><span class="sxs-lookup"><span data-stu-id="fe68d-343">ELMAH</span></span>](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a><span data-ttu-id="fe68d-344">謝誌</span><span class="sxs-lookup"><span data-stu-id="fe68d-344">Acknowledgements</span></span>

<span data-ttu-id="fe68d-345">我想要感謝下列人士提出重大貢獻的內容，本教學課程系列：</span><span class="sxs-lookup"><span data-stu-id="fe68d-345">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="fe68d-346">Alberto Poblacion、 MVP &amp; MCT，西班牙</span><span class="sxs-lookup"><span data-stu-id="fe68d-346">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="fe68d-347">[Alex Thissen，荷蘭](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))</span><span class="sxs-lookup"><span data-stu-id="fe68d-347">[Alex Thissen, Netherlands](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))</span></span>
- [<span data-ttu-id="fe68d-348">Andre Tournier，USA</span><span class="sxs-lookup"><span data-stu-id="fe68d-348">Andre Tournier, USA</span></span>](http://andret503.wordpress.com/)
- <span data-ttu-id="fe68d-349">Apurva Joshi 撰寫 Microsoft</span><span class="sxs-lookup"><span data-stu-id="fe68d-349">Apurva Joshi, Microsoft</span></span>
- [<span data-ttu-id="fe68d-350">Bojan Vrhovnik，斯洛維尼亞</span><span class="sxs-lookup"><span data-stu-id="fe68d-350">Bojan Vrhovnik, Slovenia</span></span>](http://twitter.com/bvrhovnik)
- <span data-ttu-id="fe68d-351">[Bruno sonnino 在此，巴西](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))</span><span class="sxs-lookup"><span data-stu-id="fe68d-351">[Bruno Sonnino, Brazil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))</span></span>
- [<span data-ttu-id="fe68d-352">Carlos dos Santos 巴西</span><span class="sxs-lookup"><span data-stu-id="fe68d-352">Carlos dos Santos, Brazil</span></span>](http://www.carloscds.net/)
- <span data-ttu-id="fe68d-353">[Dave Campbell，USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))</span><span class="sxs-lookup"><span data-stu-id="fe68d-353">[Dave Campbell, USA](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))</span></span>
- <span data-ttu-id="fe68d-354">[Jon Galloway，Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="fe68d-354">[Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- <span data-ttu-id="fe68d-355">[Michael 升記號，美國](http://www.930solutions.com/)(twitter: [ @mrsharps ](http://twitter.com/mrsharps))</span><span class="sxs-lookup"><span data-stu-id="fe68d-355">[Michael Sharps, USA](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))</span></span>
- <span data-ttu-id="fe68d-356">Mike 主教</span><span class="sxs-lookup"><span data-stu-id="fe68d-356">Mike Pope</span></span>
- <span data-ttu-id="fe68d-357">[Mitchel 賣方，美國](http://www.mitchelsellers.com/)(twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))</span><span class="sxs-lookup"><span data-stu-id="fe68d-357">[Mitchel Sellers, USA](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))</span></span>
- [<span data-ttu-id="fe68d-358">Paul Cociuba Microsoft</span><span class="sxs-lookup"><span data-stu-id="fe68d-358">Paul Cociuba, Microsoft</span></span>](http://linqto.me/Links/pcociuba)
- [<span data-ttu-id="fe68d-359">聖保羅 Morgado 葡萄牙</span><span class="sxs-lookup"><span data-stu-id="fe68d-359">Paulo Morgado, Portugal</span></span>](http://paulomorgado.net/)
- [<span data-ttu-id="fe68d-360">請參閱 Pranav Rastogi Microsoft</span><span class="sxs-lookup"><span data-stu-id="fe68d-360">Pranav Rastogi, Microsoft</span></span>](https://blogs.msdn.com/b/pranav_rastogi)
- [<span data-ttu-id="fe68d-361">Tim Ammann Microsoft</span><span class="sxs-lookup"><span data-stu-id="fe68d-361">Tim Ammann, Microsoft</span></span>](https://blogs.iis.net/timamm/default.aspx)
- [<span data-ttu-id="fe68d-362">Tom Dykstra Microsoft</span><span class="sxs-lookup"><span data-stu-id="fe68d-362">Tom Dykstra, Microsoft</span></span>](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a><span data-ttu-id="fe68d-363">社群投稿</span><span class="sxs-lookup"><span data-stu-id="fe68d-363">Community Contributions</span></span>

- <span data-ttu-id="fe68d-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span><span class="sxs-lookup"><span data-stu-id="fe68d-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span></span>  
  <span data-ttu-id="fe68d-365">Visual Studio 2012 相關的 MSDN 上的程式碼範例：[導覽 Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span><span class="sxs-lookup"><span data-stu-id="fe68d-365">Visual Studio 2012 related code sample on MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span></span>
- <span data-ttu-id="fe68d-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span><span class="sxs-lookup"><span data-stu-id="fe68d-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span></span>  
  <span data-ttu-id="fe68d-367">Visual Studio 2012 相關的 MSDN 上的程式碼範例： [ASP.NET 4.5 Web Form 教學課程系列中 Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span><span class="sxs-lookup"><span data-stu-id="fe68d-367">Visual Studio 2012 related code sample on MSDN: [ASP.NET 4.5 Web Forms Tutorial Series in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span></span>
- <span data-ttu-id="fe68d-368">Andrielle Azevedo-Microsoft 技術的對象參與者 (twitter: @driazevedo)</span><span class="sxs-lookup"><span data-stu-id="fe68d-368">Andrielle Azevedo - Microsoft Technical Audience Contributor (twitter: @driazevedo)</span></span>  
  <span data-ttu-id="fe68d-369">Visual Studio 2012 轉譯： [Iniciando com Visão Geral 的 ASP.NET Web Form 4.5-Parte 1-Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span><span class="sxs-lookup"><span data-stu-id="fe68d-369">Visual Studio 2012 translation: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fe68d-370">上一步</span><span class="sxs-lookup"><span data-stu-id="fe68d-370">Previous</span></span>](url-routing.md)
