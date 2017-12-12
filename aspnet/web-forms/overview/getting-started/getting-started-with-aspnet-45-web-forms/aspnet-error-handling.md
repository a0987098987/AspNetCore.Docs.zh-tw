---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: "ASP.NET 錯誤處理 |Microsoft 文件"
author: Erikre
description: "此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d5d89a6a82c91b915d61ddc3c350ea0935511c07
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-error-handling"></a><span data-ttu-id="b9ac2-103">ASP.NET 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b9ac2-103">ASP.NET Error Handling</span></span>
====================
<span data-ttu-id="b9ac2-104">由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="b9ac2-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="b9ac2-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="b9ac2-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="b9ac2-106">此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="b9ac2-107">Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="b9ac2-108">在本教學課程中，您將修改 Wingtip Toys 範例應用程式，包括錯誤處理與錯誤記錄。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-108">In this tutorial, you will modify the Wingtip Toys sample application to include error handling and error logging.</span></span> <span data-ttu-id="b9ac2-109">錯誤處理可讓應用程式正常地處理錯誤，並據以顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-109">Error handling will allow the application to gracefully handle errors and display error messages accordingly.</span></span> <span data-ttu-id="b9ac2-110">錯誤記錄可讓您找出並修正所發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-110">Error logging will allow you to find and fix errors that have occurred.</span></span> <span data-ttu-id="b9ac2-111">本教學課程先前的教學課程 」 的 URL 路由 」 為基礎，並是 Wingtip Toys 教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-111">This tutorial builds on the previous tutorial "URL Routing" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="b9ac2-112">您將學習：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-112">What you'll learn:</span></span>

- <span data-ttu-id="b9ac2-113">如何加入全域錯誤處理應用程式的組態。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-113">How to add global error handling to the application's configuration.</span></span>
- <span data-ttu-id="b9ac2-114">如何新增錯誤處理應用程式、 頁面和程式碼層級。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-114">How to add error handling at the application, page, and code levels.</span></span>
- <span data-ttu-id="b9ac2-115">如何記錄錯誤，以便稍後進行檢閱。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-115">How to log errors for later review.</span></span>
- <span data-ttu-id="b9ac2-116">如何顯示不危及安全性的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-116">How to display error messages that do not compromise security.</span></span>
- <span data-ttu-id="b9ac2-117">如何實作錯誤記錄模組和處理常式 (ELMAH) 錯誤記錄。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-117">How to implement Error Logging Modules and Handlers (ELMAH) error logging.</span></span>

## <a name="overview"></a><span data-ttu-id="b9ac2-118">概觀</span><span class="sxs-lookup"><span data-stu-id="b9ac2-118">Overview</span></span>

<span data-ttu-id="b9ac2-119">ASP.NET 應用程式必須能夠處理以一致的方式在執行期間發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-119">ASP.NET applications must be able to handle errors that occur during execution in a consistent manner.</span></span> <span data-ttu-id="b9ac2-120">ASP.NET 會使用 common language runtime (CLR)，它提供統一的方式通知應用程式的錯誤方法。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-120">ASP.NET uses the common language runtime (CLR), which provides a way of notifying applications of errors in a uniform way.</span></span> <span data-ttu-id="b9ac2-121">發生錯誤時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-121">When an error occurs, an exception is thrown.</span></span> <span data-ttu-id="b9ac2-122">任何錯誤，條件或未預期的行為，應用程式所遇到之例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-122">An exception is any error, condition, or unexpected behavior that an application encounters.</span></span>

<span data-ttu-id="b9ac2-123">在 .NET Framework 中，例外狀況是從 `System.Exception` 類別繼承的物件。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-123">In the .NET Framework, an exception is an object that inherits from the `System.Exception` class.</span></span> <span data-ttu-id="b9ac2-124">從發生問題的程式碼區域擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-124">An exception is thrown from an area of code where a problem has occurred.</span></span> <span data-ttu-id="b9ac2-125">在呼叫堆疊上傳遞的例外狀況，其中應用程式提供處理例外狀況的程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-125">The exception is passed up the call stack to a place where the application provides code to handle the exception.</span></span> <span data-ttu-id="b9ac2-126">如果應用程式不會處理例外狀況，則瀏覽器會強制顯示錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-126">If the application does not handle the exception, the browser is forced to display the error details.</span></span>

<span data-ttu-id="b9ac2-127">最佳做法，在中處理錯誤的程式碼層級`Try` / `Catch` / `Finally`區塊程式碼中的。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-127">As a best practice, handle errors in at the code level in `Try`/`Catch`/`Finally` blocks within your code.</span></span> <span data-ttu-id="b9ac2-128">嘗試將這些區塊，讓使用者可以修正其發生的內容中的問題。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-128">Try to place these blocks so that the user can correct problems in the context in which they occur.</span></span> <span data-ttu-id="b9ac2-129">如果錯誤處理區塊太遠從哪裡發生錯誤，其困難度會為使用者提供所需的資訊來修正問題。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-129">If the error handling blocks are too far away from where the error occurred, it becomes more difficult to provide users with the information they need to fix the problem.</span></span>

### <a name="exception-class"></a><span data-ttu-id="b9ac2-130">例外狀況類別</span><span class="sxs-lookup"><span data-stu-id="b9ac2-130">Exception Class</span></span>

<span data-ttu-id="b9ac2-131">例外狀況類別是從它繼承例外狀況的基底類別。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-131">The Exception class is the base class from which exceptions inherit.</span></span> <span data-ttu-id="b9ac2-132">大部分的例外狀況物件是例外狀況類別，衍生類別的執行個體，例如`SystemException`類別`IndexOutOfRangeException`類別，或`ArgumentNullException`類別。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-132">Most exception objects are instances of some derived class of the Exception class, such as the `SystemException` class, the `IndexOutOfRangeException` class, or the `ArgumentNullException` class.</span></span> <span data-ttu-id="b9ac2-133">此例外狀況類別的屬性，例如`StackTrace`屬性，`InnerException`屬性，而`Message` 屬性，提供所發生之錯誤的特定資訊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-133">The Exception class has properties, such as the `StackTrace` property, the `InnerException` property, and the `Message` property, that provide specific information about the error that has occurred.</span></span>

### <a name="exception-inheritance-hierarchy"></a><span data-ttu-id="b9ac2-134">例外狀況的繼承階層</span><span class="sxs-lookup"><span data-stu-id="b9ac2-134">Exception Inheritance Hierarchy</span></span>

<span data-ttu-id="b9ac2-135">執行階段具有一組基本的例外狀況衍生自`SystemException`遇到例外狀況時，會擲回執行階段的類別。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-135">The runtime has a base set of exceptions deriving from the `SystemException` class that the runtime throws when an exception is encountered.</span></span> <span data-ttu-id="b9ac2-136">大部分的類別繼承自例外狀況類別，例如`IndexOutOfRangeException`類別和`ArgumentNullException`類別中，不會實作其他成員。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-136">Most of the classes that inherit from the Exception class, such as the `IndexOutOfRangeException` class and the `ArgumentNullException` class, do not implement additional members.</span></span> <span data-ttu-id="b9ac2-137">因此，最重要例外狀況的資訊可以找到階層中的例外狀況、 例外狀況名稱，以及例外狀況中所包含的資訊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-137">Therefore, the most important information for an exception can be found in the hierarchy of exceptions, the exception name, and the information contained in the exception.</span></span>

### <a name="exception-handling-hierarchy"></a><span data-ttu-id="b9ac2-138">例外狀況處理的階層</span><span class="sxs-lookup"><span data-stu-id="b9ac2-138">Exception Handling Hierarchy</span></span>

<span data-ttu-id="b9ac2-139">ASP.NET Web Form 應用程式中，可以根據特定處理階層來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-139">In an ASP.NET Web Forms application, exceptions can be handled based on a specific handling hierarchy.</span></span> <span data-ttu-id="b9ac2-140">下列層級，可以處理的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-140">An exception can be handled at the following levels:</span></span>

- <span data-ttu-id="b9ac2-141">應用程式層級</span><span class="sxs-lookup"><span data-stu-id="b9ac2-141">Application level</span></span>
- <span data-ttu-id="b9ac2-142">頁面層級</span><span class="sxs-lookup"><span data-stu-id="b9ac2-142">Page level</span></span>
- <span data-ttu-id="b9ac2-143">程式碼層級</span><span class="sxs-lookup"><span data-stu-id="b9ac2-143">Code level</span></span>

<span data-ttu-id="b9ac2-144">當應用程式處理例外狀況時，都繼承自 Exception 類別的例外狀況的其他資訊可以通常擷取並顯示給使用者。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-144">When an application handles exceptions, additional information about the exception that is inherited from the Exception class can often be retrieved and displayed to the user.</span></span> <span data-ttu-id="b9ac2-145">除了應用程式、 頁面和程式碼層級，您也可以處理例外狀況 HTTP 模組層級，與使用 IIS 的自訂處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-145">In addition to application, page, and code level, you can also handle exceptions at the HTTP module level and by using an IIS custom handler.</span></span>

### <a name="application-level-error-handling"></a><span data-ttu-id="b9ac2-146">應用程式層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b9ac2-146">Application Level Error Handling</span></span>

<span data-ttu-id="b9ac2-147">您可以處理應用程式層級的預設錯誤，修改您的應用程式組態或是加入`Application_Error`中的處理常式*Global.asax*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-147">You can handle default errors at the application level either by modifying your application's configuration or by adding an `Application_Error` handler in the *Global.asax* file of your application.</span></span>

<span data-ttu-id="b9ac2-148">您可以藉由新增處理預設錯誤和 HTTP 錯誤`customErrors`區段*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-148">You can handle default errors and HTTP errors by adding a `customErrors` section to the *Web.config* file.</span></span> <span data-ttu-id="b9ac2-149">`customErrors`區段可讓您指定的使用者會被重新導向到發生錯誤時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-149">The `customErrors` section allows you to specify a default page that users will be redirected to when an error occurs.</span></span> <span data-ttu-id="b9ac2-150">它也可讓您指定的特定狀態的程式碼錯誤的個別頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-150">It also allows you to specify individual pages for specific status code errors.</span></span>

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

<span data-ttu-id="b9ac2-151">不幸的是，當您將使用者重新導向至其他頁面使用的組態，您並沒有發生之錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-151">Unfortunately, when you use the configuration to redirect the user to a different page, you do not have the details of the error that occurred.</span></span>

<span data-ttu-id="b9ac2-152">不過，將程式碼加入應用程式中攔截的任何地方發生的錯誤`Application_Error`中的處理常式*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-152">However, you can trap errors that occur anywhere in your application by adding code to the `Application_Error` handler in the *Global.asax* file.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a><span data-ttu-id="b9ac2-153">頁面層級錯誤的事件處理</span><span class="sxs-lookup"><span data-stu-id="b9ac2-153">Page Level Error Event Handling</span></span>

<span data-ttu-id="b9ac2-154">頁面層級的處理常式傳回使用者的網頁發生錯誤，但不是會維護控制項的執行個體，將不會再有任何項目頁面上。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-154">A page-level handler returns the user to the page where the error occurred, but because instances of controls are not maintained, there will no longer be anything on the page.</span></span> <span data-ttu-id="b9ac2-155">若要提供錯誤詳細資料給應用程式的使用者，必須特別的錯誤詳細資料寫入頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-155">To provide the error details to the user of the application, you must specifically write the error details to the page.</span></span>

<span data-ttu-id="b9ac2-156">記錄未處理的錯誤，或將使用者導向至可顯示有用資訊的頁面，您通常會使用頁面層級的錯誤處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-156">You would typically use a page-level error handler to log unhandled errors or to take the user to a page that can display helpful information.</span></span>

<span data-ttu-id="b9ac2-157">這個程式碼範例會顯示 ASP.NET Web 網頁中的錯誤事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-157">This code example shows a handler for the Error event in an ASP.NET Web page.</span></span> <span data-ttu-id="b9ac2-158">此處理常式會攔截所有尚未內處理的例外狀況`try` / `catch`封鎖網頁中。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-158">This handler catches all exceptions that are not already handled within `try`/`catch` blocks in the page.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

<span data-ttu-id="b9ac2-159">處理的錯誤之後，必須清除藉由呼叫`ClearError`伺服器物件的方法 (`HttpServerUtility`類別)，否則您會看到先前已發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-159">After you handle an error, you must clear it by calling the `ClearError` method of the Server object (`HttpServerUtility` class), otherwise you will see an error that has previously occurred.</span></span>

### <a name="code-level-error-handling"></a><span data-ttu-id="b9ac2-160">程式碼層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b9ac2-160">Code Level Error Handling</span></span>

<span data-ttu-id="b9ac2-161">Try-catch 陳述式包含 try 區塊後面接著一個或多個 catch 子句，指定不同的例外狀況處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-161">The try-catch statement consists of a try block followed by one or more catch clauses, which specify handlers for different exceptions.</span></span> <span data-ttu-id="b9ac2-162">擲回例外狀況時，common language runtime (CLR) 會尋找處理此例外狀況的 catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-162">When an exception is thrown, the common language runtime (CLR) looks for the catch statement that handles this exception.</span></span> <span data-ttu-id="b9ac2-163">如果目前執行的方法不會包含在 catch 區塊，CLR 就會查看目前的方法，並在呼叫堆疊上呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-163">If the currently executing method does not contain a catch block, the CLR looks at the method that called the current method, and so on, up the call stack.</span></span> <span data-ttu-id="b9ac2-164">如果找不到任何的 catch 區塊，CLR 就向使用者顯示未處理的例外狀況訊息檔案，並停止程式執行中。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-164">If no catch block is found, then the CLR displays an unhandled exception message to the user and stops execution of the program.</span></span>

<span data-ttu-id="b9ac2-165">下列程式碼範例顯示使用的常見方式`try` / `catch` / `finally`來處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-165">The following code example shows a common way of using `try`/`catch`/`finally` to handle errors.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

<span data-ttu-id="b9ac2-166">在上述程式碼中，try 區塊包含必須防範可能的例外狀況的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-166">In the above code, the try block contains the code that needs to be guarded against a possible exception.</span></span> <span data-ttu-id="b9ac2-167">區塊會執行直到擲回例外狀況或區塊已順利完成。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-167">The block is executed until either an exception is thrown or the block is completed successfully.</span></span> <span data-ttu-id="b9ac2-168">如果有任一個`FileNotFoundException`例外狀況或`IOException`發生例外狀況，執行會傳輸至其他頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-168">If either a `FileNotFoundException` exception or an `IOException` exception occurs, the execution is transferred to a different page.</span></span> <span data-ttu-id="b9ac2-169">然後，程式碼中包含最後區塊執行時，是否或未發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-169">Then, the code contained in the finally block is executed, whether an error occurred or not.</span></span>

## <a name="adding-error-logging-support"></a><span data-ttu-id="b9ac2-170">將錯誤記錄支援</span><span class="sxs-lookup"><span data-stu-id="b9ac2-170">Adding Error Logging Support</span></span>

<span data-ttu-id="b9ac2-171">之前加入錯誤處理 Wingtip Toys 範例應用程式，您將新增錯誤記錄的支援，藉由新增`ExceptionUtility`類別*邏輯*資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-171">Before adding error handling to the Wingtip Toys sample application, you will add error logging support by adding an `ExceptionUtility` class to the *Logic* folder.</span></span> <span data-ttu-id="b9ac2-172">如此一來，的每當應用程式處理錯誤，錯誤詳細資料加入至錯誤記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-172">By doing this, each time the application handles an error, the error details will be added to the error log file.</span></span>

1. <span data-ttu-id="b9ac2-173">以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-173">Right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="b9ac2-174">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-174">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="b9ac2-175">選取**Visual C#**  - &gt; **程式碼**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-175">Select the **Visual C#** -&gt; **Code** templates group on the left.</span></span> <span data-ttu-id="b9ac2-176">然後，選取**類別**中間清單並將其命名**ExceptionUtility.cs**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-176">Then, select **Class**from the middle list and name it **ExceptionUtility.cs**.</span></span>
3. <span data-ttu-id="b9ac2-177">選擇 [新增]。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-177">Choose **Add**.</span></span> <span data-ttu-id="b9ac2-178">會顯示新的類別檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-178">The new class file is displayed.</span></span>
4. <span data-ttu-id="b9ac2-179">將現有的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-179">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

<span data-ttu-id="b9ac2-180">當發生例外狀況時，例外狀況就寫入至例外狀況記錄檔藉由呼叫`LogException`方法。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-180">When an exception occurs, the exception can be written to an exception log file by calling the `LogException` method.</span></span> <span data-ttu-id="b9ac2-181">這個方法會採用兩個參數、 例外狀況物件和字串，包含有關例外狀況來源的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-181">This method takes two parameters, the exception object and a string containing details about the source of the exception.</span></span> <span data-ttu-id="b9ac2-182">例外狀況記錄會寫入*ErrorLog.txt*檔案*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-182">The exception log is written to the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>

### <a name="adding-an-error-page"></a><span data-ttu-id="b9ac2-183">加入錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="b9ac2-183">Adding an Error Page</span></span>

<span data-ttu-id="b9ac2-184">Wingtip Toys 範例應用程式，在一頁會用於顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-184">In the Wingtip Toys sample application, one page will be used to display errors.</span></span> <span data-ttu-id="b9ac2-185">錯誤頁面被設計來顯示網站的使用者安全的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-185">The error page is designed to show a secure error message to users of the site.</span></span> <span data-ttu-id="b9ac2-186">不過，如果使用者在提出 HTTP 要求提供服務時在本機電腦上的程式碼所在的開發人員，其他錯誤詳細資料會顯示錯誤頁面上。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-186">However, if the user is a developer making an HTTP request that is being served locally on the machine where the code lives, additional error details will be displayed on the error page.</span></span>

1. <span data-ttu-id="b9ac2-187">以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管 中**選取**新增** - &gt; **新項目**.</span><span class="sxs-lookup"><span data-stu-id="b9ac2-187">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="b9ac2-188">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-188">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="b9ac2-189">選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-189">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="b9ac2-190">從 [中間] 清單中選取**使用主版頁面的 Web Form**，並將其命名**ErrorPage.aspx**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-190">From the middle list, select **Web Form with Master Page**,and name it **ErrorPage.aspx**.</span></span>
3. <span data-ttu-id="b9ac2-191">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-191">Click **Add**.</span></span>
4. <span data-ttu-id="b9ac2-192">選取*Site.Master*主版頁面中，為檔案，然後選擇 **確定**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-192">Select the *Site.Master* file as the master page, and then choose **OK**.</span></span>
5. <span data-ttu-id="b9ac2-193">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-193">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. <span data-ttu-id="b9ac2-194">取代現有的程式碼的程式碼後置 (*ErrorPage.aspx.cs*) 使其顯示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-194">Replace the existing code of the code-behind (*ErrorPage.aspx.cs*) so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

<span data-ttu-id="b9ac2-195">顯示錯誤頁面時，`Page_Load`事件處理常式會執行。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-195">When the error page is displayed, the `Page_Load` event handler is executed.</span></span> <span data-ttu-id="b9ac2-196">在`Page_Load`處理常式，先處理此錯誤的位置來決定。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-196">In the `Page_Load` handler, the location of where the error was first handled is determined.</span></span> <span data-ttu-id="b9ac2-197">然後，最後發生的錯誤由呼叫`GetLastError`伺服器物件的方法。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-197">Then, the last error that occurred is determined by call the `GetLastError` method of the Server object.</span></span> <span data-ttu-id="b9ac2-198">如果例外狀況不再存在，則會建立泛型例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-198">If the exception no longer exists, a generic exception is created.</span></span> <span data-ttu-id="b9ac2-199">接著，如果已在本機發出的 HTTP 要求，會顯示所有的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-199">Then, if the HTTP request was made locally, all error details are shown.</span></span> <span data-ttu-id="b9ac2-200">在此情況下，只有本機電腦執行 web 應用程式會看到這些錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-200">In this case, only the local machine running the web application will see these error details.</span></span> <span data-ttu-id="b9ac2-201">已經顯示錯誤資訊之後，錯誤會加入至記錄檔，並從伺服器會清除錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-201">After the error information has been displayed, the error is added to the log file and the error is cleared from the server.</span></span>

### <a name="displaying-unhandled-error-messages-for-the-application"></a><span data-ttu-id="b9ac2-202">顯示應用程式的未處理的錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="b9ac2-202">Displaying Unhandled Error Messages for the Application</span></span>

<span data-ttu-id="b9ac2-203">藉由新增`customErrors`區段*Web.config*檔案中，您可以快速處理簡單整個應用程式發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-203">By adding a `customErrors` section to the *Web.config* file, you can quickly handle simple errors that occur throughout the application.</span></span> <span data-ttu-id="b9ac2-204">您也可以指定如何處理根據其狀態的程式碼值，例如，404-找不到檔案的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-204">You can also specify how to handle errors based on their status code value, such as 404 - File not found.</span></span>

#### <a name="update-the-configuration"></a><span data-ttu-id="b9ac2-205">更新設定</span><span class="sxs-lookup"><span data-stu-id="b9ac2-205">Update the Configuration</span></span>

<span data-ttu-id="b9ac2-206">更新組態，藉由新增`customErrors`區段*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-206">Update the configuration by adding a `customErrors` section to the *Web.config* file.</span></span>

1. <span data-ttu-id="b9ac2-207">在**方案總管 中**、 尋找和開啟*Web.config* Wingtip Toys 範例應用程式根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-207">In **Solution Explorer**, find and open the *Web.config* file at the root of the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="b9ac2-208">新增`customErrors`區段*Web.config*檔案內`<system.web>`節點，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-208">Add the `customErrors` section to the *Web.config* file within the `<system.web>` node as follows:</span></span>   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. <span data-ttu-id="b9ac2-209">儲存*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-209">Save the *Web.config* file.</span></span>

<span data-ttu-id="b9ac2-210">`customErrors`區段會指定模式中，設定為 「 On 」。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-210">The `customErrors` section specifies the mode, which is set to "On".</span></span> <span data-ttu-id="b9ac2-211">它也會指定`defaultRedirect`，它會告訴應用程式發生錯誤時，瀏覽至哪一頁。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-211">It also specifies the `defaultRedirect`, which tells the application which page to navigate to when an error occurs.</span></span> <span data-ttu-id="b9ac2-212">此外，您已加入指定如何處理 404 錯誤，找不到頁面時的特定錯誤項目。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-212">In addition, you have added a specific error element that specifies how to handle a 404 error when a page is not found.</span></span> <span data-ttu-id="b9ac2-213">稍後在本教學課程中，您將加入其他錯誤處理，將擷取的應用程式層級的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-213">Later in this tutorial, you will add additional error handling that will capture the details of an error at the application level.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="b9ac2-214">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b9ac2-214">Running the Application</span></span>

<span data-ttu-id="b9ac2-215">您可以執行應用程式現在以查看更新的路由。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-215">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="b9ac2-216">按**F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-216">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="b9ac2-217">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-217">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="b9ac2-218">在瀏覽器中輸入下列 URL (請務必使用**您**連接埠號碼):</span><span class="sxs-lookup"><span data-stu-id="b9ac2-218">Enter the following URL into the browser (be sure to use **your** port number):</span></span>  
    `https://localhost:44300/NoPage.aspx`
3. <span data-ttu-id="b9ac2-219">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-219">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理-找不到網頁時發生錯誤](aspnet-error-handling/_static/image1.png)

<span data-ttu-id="b9ac2-221">當您要求*NoPage.aspx*頁面上，不存在，錯誤頁面就會顯示簡單的錯誤訊息以及詳細的錯誤資訊有提供其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-221">When you request the *NoPage.aspx* page, which does not exist, the error page will show the simple error message and the detailed error information if additional details are available.</span></span> <span data-ttu-id="b9ac2-222">不過，如果使用者從遠端位置要求不存在的頁面上，[錯誤] 頁面會只有錯誤訊息以紅色顯示。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-222">However, if the user requested a non-existent page from a remote location, the error page would only show the error message in red.</span></span>

### <a name="including-an-exception-for-testing-purposes"></a><span data-ttu-id="b9ac2-223">基於測試目的包括例外狀況</span><span class="sxs-lookup"><span data-stu-id="b9ac2-223">Including an Exception for Testing Purposes</span></span>

<span data-ttu-id="b9ac2-224">若要確認您的應用程式的運作方式錯誤時，就會發生，在 ASP.NET 中刻意建立錯誤條件。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-224">To verify how your application will function when an error occurs, you can deliberately create error conditions in ASP.NET.</span></span> <span data-ttu-id="b9ac2-225">在 Wingtip Toys 範例應用程式，您會擲回測試例外狀況時若要查看發生的事載入預設頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-225">In the Wingtip Toys sample application, you will throw a test exception when the default page loads to see what happens.</span></span>

1. <span data-ttu-id="b9ac2-226">開啟 程式碼後置的*Default.aspx* Visual Studio 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-226">Open the code-behind of the *Default.aspx* page in Visual Studio.</span></span>   
 <span data-ttu-id="b9ac2-227">*Default.aspx.cs*將會顯示程式碼後置頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-227">The *Default.aspx.cs* code-behind page will be displayed.</span></span>
2. <span data-ttu-id="b9ac2-228">在`Page_Load`處理常式，加入程式碼，這樣的處理常式隨即出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-228">In the `Page_Load` handler, add code so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

<span data-ttu-id="b9ac2-229">您可建立各種不同的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-229">It is possible to create various different types of exceptions.</span></span> <span data-ttu-id="b9ac2-230">在上述程式碼中，您將建立`InvalidOperationException`時*Default.aspx*載入頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-230">In the above code, you are creating an `InvalidOperationException` when the *Default.aspx* page is loaded.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="b9ac2-231">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b9ac2-231">Running the Application</span></span>

<span data-ttu-id="b9ac2-232">您可以執行的應用程式，以查看應用程式如何處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-232">You can run the application to see how the application handles the exception.</span></span>

1. <span data-ttu-id="b9ac2-233">按**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-233">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="b9ac2-234">應用程式會擲回 InvalidOperationException。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-234">The application throws the InvalidOperationException.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="b9ac2-235">您必須按**CTRL + F5**顯示頁面，而不會中斷程式碼，在 Visual Studio 中檢視錯誤的來源。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-235">You must press **CTRL+F5** to display the page without breaking into the code to view the source of the error in Visual Studio.</span></span>
2. <span data-ttu-id="b9ac2-236">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-236">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理-錯誤頁面](aspnet-error-handling/_static/image2.png)

<span data-ttu-id="b9ac2-238">您可以看到錯誤詳細資料中，例外狀況已受困於連結`customError`一節中*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-238">As you can see in the error details, the exception was trapped by the `customError` section in the *Web.config* file.</span></span>

### <a name="adding-application-level-error-handling"></a><span data-ttu-id="b9ac2-239">加入應用程式層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b9ac2-239">Adding Application-Level Error Handling</span></span>

<span data-ttu-id="b9ac2-240">而不是設陷例外狀況使用`customErrors`一節中*Web.config*檔案中，在您取得少數例外狀況的相關資訊，您可以捕捉此錯誤，應用程式層級，並擷取錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-240">Rather than trap the exception using the `customErrors` section in the *Web.config* file, where you gain little information about the exception, you can trap the error at the application level and retrieve error details.</span></span>

1. <span data-ttu-id="b9ac2-241">在**方案總管 中**、 尋找和開啟*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-241">In **Solution Explorer**, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="b9ac2-242">新增**應用程式\_錯誤**處理常式，使其顯示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-242">Add an **Application\_Error** handler so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

<span data-ttu-id="b9ac2-243">應用程式時，發生錯誤時`Application_Error`會呼叫處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-243">When an error occurs in the application, the `Application_Error` handler is called.</span></span> <span data-ttu-id="b9ac2-244">這個處理常式的最後一個例外狀況是擷取，然後檢閱。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-244">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="b9ac2-245">如果未處理的例外狀況和例外狀況包含內部例外狀況詳細資料 (也就是`InnerException`不是 null)，應用程式將執行轉移至錯誤網頁顯示例外狀況詳細資料的位置。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-245">If the exception was unhandled and the exception contains inner-exception details (that is, `InnerException` is not null), the application transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="b9ac2-246">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b9ac2-246">Running the Application</span></span>

<span data-ttu-id="b9ac2-247">您可以執行應用程式所處理的例外狀況，應用程式層級提供的其他錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-247">You can run the application to see the additional error details provided by handling the exception at the application level.</span></span>

1. <span data-ttu-id="b9ac2-248">按**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-248">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="b9ac2-249">應用程式會擲回`InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-249">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="b9ac2-250">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-250">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理-應用程式層級錯誤](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a><span data-ttu-id="b9ac2-252">加入頁面層級的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b9ac2-252">Adding Page-Level Error Handling</span></span>

<span data-ttu-id="b9ac2-253">您可以加入頁面層級的錯誤處理頁面使用 新增`ErrorPage`屬性`@Page`指示詞的頁面上，或藉由新增`Page_Error`程式碼後置頁面的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-253">You can add page-level error handling to a page either by using adding an `ErrorPage` attribute to the `@Page` directive of the page, or by adding a `Page_Error` event handler to the code-behind of a page.</span></span> <span data-ttu-id="b9ac2-254">在本節中，您將加入`Page_Error`會傳輸到執行的事件處理常式*ErrorPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-254">In this section, you will add a `Page_Error` event handler that will transfer execution to the *ErrorPage.aspx* page.</span></span>

1. <span data-ttu-id="b9ac2-255">在**方案總管 中**、 尋找和開啟*Default.aspx.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-255">In **Solution Explorer**, find and open the *Default.aspx.cs* file.</span></span>
2. <span data-ttu-id="b9ac2-256">新增`Page_Error`處理常式會使程式碼後置會顯示為遵循：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-256">Add a `Page_Error` handler so that the code-behind appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

<span data-ttu-id="b9ac2-257">在頁面上，發生錯誤時`Page_Error`會呼叫事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-257">When an error occurs on the page, the `Page_Error` event handler is called.</span></span> <span data-ttu-id="b9ac2-258">這個處理常式的最後一個例外狀況是擷取，然後檢閱。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-258">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="b9ac2-259">如果`InvalidOperationException`發生，`Page_Error`事件處理常式將執行轉移至錯誤網頁顯示例外狀況詳細資料的位置。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-259">If an `InvalidOperationException` occurs, the `Page_Error` event handler transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="b9ac2-260">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b9ac2-260">Running the Application</span></span>

<span data-ttu-id="b9ac2-261">您可以執行應用程式現在以查看更新的路由。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-261">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="b9ac2-262">按**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-262">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="b9ac2-263">應用程式會擲回`InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-263">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="b9ac2-264">檢閱*ErrorPage.aspx*瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-264">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET 錯誤處理的頁面層級錯誤](aspnet-error-handling/_static/image4.png)
3. <span data-ttu-id="b9ac2-266">關閉瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-266">Close your browser window.</span></span>

### <a name="removing-the-exception-used-for-testing"></a><span data-ttu-id="b9ac2-267">移除用於測試的例外狀況</span><span class="sxs-lookup"><span data-stu-id="b9ac2-267">Removing the Exception Used for Testing</span></span>

<span data-ttu-id="b9ac2-268">若要讓 Wingtip Toys 範例應用程式，而不擲回的例外狀況，您稍早在本教學課程中加入函式，移除例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-268">To allow the Wingtip Toys sample application to function without throwing the exception you added earlier in this tutorial, remove the exception.</span></span>

1. <span data-ttu-id="b9ac2-269">開啟 程式碼後置的*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-269">Open the code-behind of the *Default.aspx* page.</span></span>
2. <span data-ttu-id="b9ac2-270">在`Page_Load`處理常式中，移除此程式碼擲回例外狀況，這樣的處理常式隨即出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-270">In the `Page_Load` handler, remove the code that throws the exception so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a><span data-ttu-id="b9ac2-271">加入程式碼層級的錯誤記錄</span><span class="sxs-lookup"><span data-stu-id="b9ac2-271">Adding Code-Level Error Logging</span></span>

<span data-ttu-id="b9ac2-272">如稍早在本教學課程中所述，您可以加入嘗試執行程式碼區段，並處理，就會發生的第一個錯誤的 try/catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-272">As mentioned earlier in this tutorial, you can add try/catch statements to attempt to run a section of code and handle the first error that occurs.</span></span> <span data-ttu-id="b9ac2-273">在此範例中，您只會寫入錯誤詳細資料的錯誤記錄檔，以便稍後可以檢閱錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-273">In this example, you will only write the error details to the error log file so that the error can be reviewed later.</span></span>

1. <span data-ttu-id="b9ac2-274">在**方案總管 中**，請在*邏輯*資料夾、 尋找和開啟*PayPalFunctions.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-274">In **Solution Explorer**, in the *Logic* folder, find and open the *PayPalFunctions.cs* file.</span></span>
2. <span data-ttu-id="b9ac2-275">更新`HttpCall`方法讓，程式碼會顯示為如下所示：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-275">Update the `HttpCall` method so that the code appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

<span data-ttu-id="b9ac2-276">上述程式碼會呼叫`LogException`方法中所包含`ExceptionUtility`類別。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-276">The above code calls the `LogException` method that is contained in the `ExceptionUtility` class.</span></span> <span data-ttu-id="b9ac2-277">您加入*ExceptionUtility.cs*類別檔案，以*邏輯*稍早在本教學課程中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-277">You added the *ExceptionUtility.cs* class file to the *Logic* folder earlier in this tutorial.</span></span> <span data-ttu-id="b9ac2-278">`LogException`方法會採用兩個參數。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-278">The `LogException` method takes two parameters.</span></span> <span data-ttu-id="b9ac2-279">第一個參數是例外狀況物件。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-279">The first parameter is the exception object.</span></span> <span data-ttu-id="b9ac2-280">第二個參數是用來識別錯誤來源的字串。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-280">The second parameter is a string used to recognize the source of the error.</span></span>

### <a name="inspecting-the-error-logging-information"></a><span data-ttu-id="b9ac2-281">檢查錯誤記錄資訊</span><span class="sxs-lookup"><span data-stu-id="b9ac2-281">Inspecting the Error Logging Information</span></span>

<span data-ttu-id="b9ac2-282">如先前所述，您可以使用錯誤記錄檔以判斷應該先修正您的應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-282">As mentioned previously, you can use the error log to determine which errors in your application should be fixed first.</span></span> <span data-ttu-id="b9ac2-283">當然，不會記錄錯誤，截取，寫入錯誤記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-283">Of course, only errors that have been trapped and written to the error log will be recorded.</span></span>

1. <span data-ttu-id="b9ac2-284">在**方案總管 中**、 尋找和開啟*ErrorLog.txt*檔案*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-284">In **Solution Explorer**, find and open the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>   
 <span data-ttu-id="b9ac2-285">您可能需要選取"**顯示所有檔案**」 選項或"**重新整理**」 選項，從頂端**方案總管 中**查看*ErrorLog.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-285">You may need to select the "**Show All Files**" option or the "**Refresh**" option from the top of **Solution Explorer** to see the *ErrorLog.txt* file.</span></span>
2. <span data-ttu-id="b9ac2-286">檢閱顯示在 Visual Studio 中的錯誤記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-286">Review the error log displayed in Visual Studio:</span></span> 

    ![ASP.NET 錯誤處理-ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a><span data-ttu-id="b9ac2-288">安全的錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="b9ac2-288">Safe Error Messages</span></span>

<span data-ttu-id="b9ac2-289">它是**很重要的一點**，當您的應用程式會顯示錯誤訊息，它不應將惡意使用者可能會發現很有幫助攻擊您的應用程式的資訊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-289">It is **important to note** that when your application displays error messages, it should not give away information that a malicious user might find helpful in attacking your application.</span></span> <span data-ttu-id="b9ac2-290">例如，如果您的應用程式未順利嘗試寫入資料庫，它不應該顯示錯誤訊息，包括它正在使用的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-290">For example, if your application unsuccessfully tries to write in to a database, it should not display an error message that includes the user name it is using.</span></span> <span data-ttu-id="b9ac2-291">基於這個理由，以紅色的一般錯誤訊息會顯示給使用者。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-291">For this reason, a generic error message in red is displayed to the user.</span></span> <span data-ttu-id="b9ac2-292">所有其他錯誤詳細資料只會顯示在本機電腦上，開發人員。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-292">All additional error details are only displayed to the developer on the local machine.</span></span>

## <a name="using-elmah"></a><span data-ttu-id="b9ac2-293">使用 ELMAH</span><span class="sxs-lookup"><span data-stu-id="b9ac2-293">Using ELMAH</span></span>

<span data-ttu-id="b9ac2-294">ELMAH （錯誤記錄模組和處理常式） 是錯誤記錄工具，您可外掛至您的 ASP.NET 應用程式，以 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-294">ELMAH (Error Logging Modules and Handlers) is an error logging facility that you plug into your ASP.NET application as a NuGet package.</span></span> <span data-ttu-id="b9ac2-295">ELMAH 提供下列功能：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-295">ELMAH provides the following capabilities:</span></span>

- <span data-ttu-id="b9ac2-296">記錄的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-296">Logging of unhandled exceptions.</span></span>
- <span data-ttu-id="b9ac2-297">若要檢視錄製之未處理的例外狀況的整個記錄檔的網頁。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-297">A web page to view the entire log of recoded unhandled exceptions.</span></span>
- <span data-ttu-id="b9ac2-298">若要檢視完整的詳細資料，每個網頁記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-298">A web page to view the full details of each logged exception.</span></span>
- <span data-ttu-id="b9ac2-299">它發生時每個錯誤的電子郵件通知。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-299">An e-mail notification of each error at the time it occurs.</span></span>
- <span data-ttu-id="b9ac2-300">從記錄檔，RSS 摘要的最後 15 個錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-300">An RSS feed of the last 15 errors from the log.</span></span>

<span data-ttu-id="b9ac2-301">您可以使用 ELMAH，您必須先安裝它。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-301">Before you can work with the ELMAH, you must install it.</span></span> <span data-ttu-id="b9ac2-302">這是容易使用*NuGet*套件安裝程式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-302">This is easy using the *NuGet* package installer.</span></span> <span data-ttu-id="b9ac2-303">如稍早在本教學課程中所述，NuGet 是 Visual Studio 擴充功能，可讓您輕鬆安裝及更新的開放原始碼程式庫和 Visual Studio 中的工具。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-303">As mentioned earlier in this tutorial series, NuGet is a Visual Studio extension that makes it easy to install and update open source libraries and tools in Visual Studio.</span></span>

1. <span data-ttu-id="b9ac2-304">在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員** - &gt; **管理方案的 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-304">Within Visual Studio, from the **Tools** menu, select **Library Package Manager** -&gt; **Manage NuGet Packages for Solution**.</span></span> 

    ![ASP.NET 錯誤處理-管理方案的 NuGet 封裝](aspnet-error-handling/_static/image6.png)
2. <span data-ttu-id="b9ac2-306">**管理 NuGet 封裝**Visual Studio 中會顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-306">The **Manage NuGet Packages** dialog box is displayed within Visual Studio.</span></span>
3. <span data-ttu-id="b9ac2-307">在**管理 NuGet 封裝**對話方塊方塊中，展開 **線上**的左側，然後選取**nuget.org**。然後，尋找並安裝**ELMAH**封裝從線上可用的封裝清單。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-307">In the **Manage NuGet Packages** dialog box, expand **Online** on the left, and then select **nuget.org**. Then, find and install the **ELMAH** package from the list of available packages online.</span></span> 

    ![ASP.NET 錯誤處理-ELMA NuGet 封裝](aspnet-error-handling/_static/image7.png)
4. <span data-ttu-id="b9ac2-309">您必須要有網際網路連線來下載套件。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-309">You will need to have an internet connection to download the package.</span></span>
5. <span data-ttu-id="b9ac2-310">在**選取專案**對話方塊方塊中，請確定**WingtipToys**選取項目已選取，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-310">In the **Select Projects** dialog box, make sure the **WingtipToys** selection is selected, and then click **OK**.</span></span> 

    ![ASP.NET 錯誤處理-選取的專案 對話方塊](aspnet-error-handling/_static/image8.png)
6. <span data-ttu-id="b9ac2-312">按一下**關閉**中**管理 NuGet 封裝**視 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-312">Click **Close** in **the Manage NuGet Packages** dialog box if needed.</span></span>
7. <span data-ttu-id="b9ac2-313">如果 Visual Studio 會要求您重新載入任何開啟的檔案，請選取 「**全部皆是**"。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-313">If Visual Studio requests that you reload any open files, select "**Yes to All**".</span></span>
8. <span data-ttu-id="b9ac2-314">ELMAH 封裝本身中加入項目*Web.config*專案根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-314">The ELMAH package adds entries for itself in the *Web.config* file at the root of your project.</span></span> <span data-ttu-id="b9ac2-315">如果 Visual Studio 會要求您如果您想要重新載入已修改*Web.config*檔案，請按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-315">If Visual Studio asks you if you want to reload the modified *Web.config* file, click **Yes**.</span></span>

<span data-ttu-id="b9ac2-316">ELMAH 現在已準備好儲存發生的任何未處理的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-316">ELMAH is now ready to store any unhandled errors that occur.</span></span>

### <a name="viewing-the-elmah-log"></a><span data-ttu-id="b9ac2-317">檢視 ELMAH 記錄檔</span><span class="sxs-lookup"><span data-stu-id="b9ac2-317">Viewing the ELMAH Log</span></span>

<span data-ttu-id="b9ac2-318">檢視 ELMAH 記錄檔很容易，但首先您要建立將 ELMAH 記錄檔中記錄未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-318">Viewing the ELMAH log is easy, but first you will create an unhandled exception that will be recorded in the ELMAH log.</span></span>

1. <span data-ttu-id="b9ac2-319">按**CTRL + F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-319">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="b9ac2-320">若要寫入 ELMAH 記錄未處理的例外狀況，請瀏覽至下列 URL （使用連接埠號碼） 瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-320">To write an unhandled exception to the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    <span data-ttu-id="b9ac2-321">`https://localhost:44300/NoPage.aspx`將會顯示錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-321">`https://localhost:44300/NoPage.aspx` The error page will be displayed.</span></span>
3. <span data-ttu-id="b9ac2-322">若要顯示 ELMAH 記錄檔，請瀏覽至下列 URL （使用連接埠號碼） 瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-322">To display the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET 錯誤處理-ELMAH 錯誤記錄檔](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="b9ac2-324">總結</span><span class="sxs-lookup"><span data-stu-id="b9ac2-324">Summary</span></span>

<span data-ttu-id="b9ac2-325">在本教學課程中，您已經學會有關在應用程式層級、 頁面層級及程式碼層級的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-325">In this tutorial, you have learned about handling errors at the application level, the page level, and the code level.</span></span> <span data-ttu-id="b9ac2-326">您也已經學會如何記錄處理和未處理的錯誤，以便稍後進行檢閱。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-326">You have also learned how to log handled and unhandled errors for later review.</span></span> <span data-ttu-id="b9ac2-327">您已新增 ELMAH 公用程式，以提供例外狀況記錄和通知給您的應用程式使用 NuGet。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-327">You added the ELMAH utility to provide exception logging and notification to your application using NuGet.</span></span> <span data-ttu-id="b9ac2-328">此外，您已經學會關於安全的錯誤訊息的重要性。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-328">Additionally, you have learned about the importance of safe error messages.</span></span>

## <a name="tutorial-series-conclusion"></a><span data-ttu-id="b9ac2-329">教學課程系列的結論</span><span class="sxs-lookup"><span data-stu-id="b9ac2-329">Tutorial Series Conclusion</span></span>

<span data-ttu-id="b9ac2-330">*感謝您沿著下列。我希望這組教學課程，幫助您更熟悉 ASP.NET Web Form。如果您需要適用於 ASP.NET 4.5 和 Visual Studio 2013 的 Web Form 功能的詳細資訊，請參閱* [ *ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊*](../../../../visual-studio/overview/2013/release-notes.md) *.此外，請務必查看本教學課程中所述*  ***接下來的步驟 * * * 區段和 defintely 試用* [*免費試用 Azure*](https://azure.microsoft.com/pricing/free-trial/)*.*</span><span class="sxs-lookup"><span data-stu-id="b9ac2-330">*Thanks for following along. I hope this set of tutorials helped you become more familiar with ASP.NET Web Forms. If you need more information about Web Forms features available in ASP.NET 4.5 and Visual Studio 2013, see* [*ASP.NET and Web Tools for Visual Studio 2013 Release Notes*](../../../../visual-studio/overview/2013/release-notes.md)*. Also, be sure to take a look at the tutorial mentioned in the* ***Next Steps****section and defintely try out the* [*free Azure trial*](https://azure.microsoft.com/pricing/free-trial/)*.*</span></span>

![感謝您-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a><span data-ttu-id="b9ac2-332">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b9ac2-332">Next Steps</span></span>

<span data-ttu-id="b9ac2-333">深入了解部署到 Microsoft Azure web 應用程式，請參閱[部署至 Azure 網站的安全 ASP.NET Web Form 應用程式成員資格、 OAuth、 與 SQL Database](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-333">Learn more about deploying your web application to Microsoft Azure, see [Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Web Site](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).</span></span>

## <a name="free-trial"></a><span data-ttu-id="b9ac2-334">免費試用版</span><span class="sxs-lookup"><span data-stu-id="b9ac2-334">Free Trial</span></span>

[<span data-ttu-id="b9ac2-335">Microsoft Azure-免費試用版</span><span class="sxs-lookup"><span data-stu-id="b9ac2-335">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)  
 <span data-ttu-id="b9ac2-336">您的網站發行到 Microsoft Azure 可節省時間、 維護和費用。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-336">Publishing your website to Microsoft Azure will save you time, maintenance and expense.</span></span> <span data-ttu-id="b9ac2-337">它是快速處理程序將您的 web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-337">It's a quick process to deploying your web app to Azure.</span></span> <span data-ttu-id="b9ac2-338">當您需要維護和監視您的 web 應用程式時，Azure 會提供各種不同的工具和服務。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-338">When you need to maintain and monitor your web app, Azure offers a variety of tools and services.</span></span> <span data-ttu-id="b9ac2-339">管理資料、 傳輸、 識別、 備份、 訊息、 媒體和在 Azure 中的效能。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-339">Manage data, traffic, identity, backups, messaging, media and performance in Azure.</span></span> <span data-ttu-id="b9ac2-340">並且，全部都提供非常符合成本效益的方法。</span><span class="sxs-lookup"><span data-stu-id="b9ac2-340">And, all of this is provided in a very cost effective approach.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9ac2-341">其他資源</span><span class="sxs-lookup"><span data-stu-id="b9ac2-341">Additional Resources</span></span>

<span data-ttu-id="b9ac2-342">[記錄錯誤的詳細資訊與 ASP.NET 健康監視](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span><span class="sxs-lookup"><span data-stu-id="b9ac2-342">[Logging Error Details with ASP.NET Health Monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span></span>  
[<span data-ttu-id="b9ac2-343">ELMAH</span><span class="sxs-lookup"><span data-stu-id="b9ac2-343">ELMAH</span></span>](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a><span data-ttu-id="b9ac2-344">謝誌</span><span class="sxs-lookup"><span data-stu-id="b9ac2-344">Acknowledgements</span></span>

<span data-ttu-id="b9ac2-345">我想要感謝下列重大貢獻內容本教學課程系列的人：</span><span class="sxs-lookup"><span data-stu-id="b9ac2-345">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="b9ac2-346">Alberto Poblacion、 MVP &amp; mct 規範、 西班牙</span><span class="sxs-lookup"><span data-stu-id="b9ac2-346">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="b9ac2-347">[Alex Thissen、 荷蘭](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-347">[Alex Thissen, Netherlands](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))</span></span>
- [<span data-ttu-id="b9ac2-348">Andre Tournier，美國</span><span class="sxs-lookup"><span data-stu-id="b9ac2-348">Andre Tournier, USA</span></span>](http://andret503.wordpress.com/)
- <span data-ttu-id="b9ac2-349">Apurva Joshi Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9ac2-349">Apurva Joshi, Microsoft</span></span>
- [<span data-ttu-id="b9ac2-350">Bojan Vrhovnik、 斯洛維尼亞</span><span class="sxs-lookup"><span data-stu-id="b9ac2-350">Bojan Vrhovnik, Slovenia</span></span>](http://twitter.com/bvrhovnik)
- <span data-ttu-id="b9ac2-351">[Bruno Sonnino，巴西](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-351">[Bruno Sonnino, Brazil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))</span></span>
- [<span data-ttu-id="b9ac2-352">Carlos dos Santos 巴西</span><span class="sxs-lookup"><span data-stu-id="b9ac2-352">Carlos dos Santos, Brazil</span></span>](http://www.carloscds.net/)
- <span data-ttu-id="b9ac2-353">[Dave Campbell，USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-353">[Dave Campbell, USA](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))</span></span>
- <span data-ttu-id="b9ac2-354">[Jon Galloway、 Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-354">[Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- <span data-ttu-id="b9ac2-355">[Michael 升記號、 美國](http://www.930solutions.com/)(twitter: [ @mrsharps ](http://twitter.com/mrsharps))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-355">[Michael Sharps, USA](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))</span></span>
- <span data-ttu-id="b9ac2-356">Mike 教宗</span><span class="sxs-lookup"><span data-stu-id="b9ac2-356">Mike Pope</span></span>
- <span data-ttu-id="b9ac2-357">[Mitchel 銷售、 美國](http://www.mitchelsellers.com/)(twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-357">[Mitchel Sellers, USA](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))</span></span>
- [<span data-ttu-id="b9ac2-358">Paul Cociuba Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9ac2-358">Paul Cociuba, Microsoft</span></span>](http://linqto.me/Links/pcociuba)
- [<span data-ttu-id="b9ac2-359">Paulo Morgado，葡萄牙</span><span class="sxs-lookup"><span data-stu-id="b9ac2-359">Paulo Morgado, Portugal</span></span>](http://paulomorgado.net/)
- [<span data-ttu-id="b9ac2-360">Pranav Rastogi Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9ac2-360">Pranav Rastogi, Microsoft</span></span>](https://blogs.msdn.com/b/pranav_rastogi)
- [<span data-ttu-id="b9ac2-361">Tim Ammann、 Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9ac2-361">Tim Ammann, Microsoft</span></span>](https://blogs.iis.net/timamm/default.aspx)
- [<span data-ttu-id="b9ac2-362">Tom Dykstra Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9ac2-362">Tom Dykstra, Microsoft</span></span>](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a><span data-ttu-id="b9ac2-363">社群投稿</span><span class="sxs-lookup"><span data-stu-id="b9ac2-363">Community Contributions</span></span>

- <span data-ttu-id="b9ac2-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span></span>  
 <span data-ttu-id="b9ac2-365">Visual Studio 2012 相關的 MSDN 上的程式碼範例：[瀏覽 Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span><span class="sxs-lookup"><span data-stu-id="b9ac2-365">Visual Studio 2012 related code sample on MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span></span>
- <span data-ttu-id="b9ac2-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span><span class="sxs-lookup"><span data-stu-id="b9ac2-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span></span>  
 <span data-ttu-id="b9ac2-367">Visual Studio 2012 相關的 MSDN 上的程式碼範例： [ASP.NET 4.5 Web Form 教學課程系列在 Visual Basic 中](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span><span class="sxs-lookup"><span data-stu-id="b9ac2-367">Visual Studio 2012 related code sample on MSDN: [ASP.NET 4.5 Web Forms Tutorial Series in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span></span>
- <span data-ttu-id="b9ac2-368">Andrielle Azevedo-Microsoft 技術的對象參與者 (twitter: @driazevedo)</span><span class="sxs-lookup"><span data-stu-id="b9ac2-368">Andrielle Azevedo - Microsoft Technical Audience Contributor (twitter: @driazevedo)</span></span>  
 <span data-ttu-id="b9ac2-369">Visual Studio 2012 轉譯： [Iniciando com Visão Geral 的 ASP.NET Web Form 4.5-Parte 1-Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span><span class="sxs-lookup"><span data-stu-id="b9ac2-369">Visual Studio 2012 translation: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b9ac2-370">上一步</span><span class="sxs-lookup"><span data-stu-id="b9ac2-370">Previous</span></span>](url-routing.md)
