---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: '反覆項目 #6 – 使用測試為導向的開發 (VB) |Microsoft 文件'
author: microsoft
description: 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 71b3425c5ca8cbfc1b89493c7afb26681f8bdc9d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="iteration-6--use-test-driven-development-vb"></a><span data-ttu-id="7d72e-104">反覆項目 #6 – 使用測試為導向的開發 (VB)</span><span class="sxs-lookup"><span data-stu-id="7d72e-104">Iteration #6 – Use test-driven development (VB)</span></span>
====================
<span data-ttu-id="7d72e-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7d72e-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="7d72e-106">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="7d72e-106">Download Code</span></span>](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> <span data-ttu-id="7d72e-107">這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-107">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="7d72e-108">在這個反覆項目，我們會加入連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-108">In this iteration, we add contact groups.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="7d72e-109">建立連絡人管理 ASP.NET MVC 應用程式 (VB)</span><span class="sxs-lookup"><span data-stu-id="7d72e-109">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="7d72e-110">在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="7d72e-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="7d72e-111">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="7d72e-112">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="7d72e-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="7d72e-113">與每個反覆項目，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="7d72e-114">這個多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="7d72e-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="7d72e-115">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="7d72e-116">在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="7d72e-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="7d72e-117">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="7d72e-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="7d72e-118">反覆項目 #2-請看起來很棒的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="7d72e-119">在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="7d72e-120">反覆項目 #3-加入表單驗證。</span><span class="sxs-lookup"><span data-stu-id="7d72e-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="7d72e-121">第三個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="7d72e-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="7d72e-122">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="7d72e-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="7d72e-123">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="7d72e-124">反覆項目 #4-請鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="7d72e-125">在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-125">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="7d72e-126">例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="7d72e-127">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="7d72e-128">第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="7d72e-129">我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="7d72e-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="7d72e-130">反覆項目 #6-使用測試為導向的開發。</span><span class="sxs-lookup"><span data-stu-id="7d72e-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="7d72e-131">這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="7d72e-132">在這個反覆項目，我們會加入連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="7d72e-133">反覆項目 #7： 加入 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="7d72e-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="7d72e-134">在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="7d72e-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="7d72e-135">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="7d72e-135">This Iteration</span></span>

<span data-ttu-id="7d72e-136">在連絡人管理員應用程式的上一個反覆項目，我們會建立單元測試，以提供我們的程式碼的安全網。</span><span class="sxs-lookup"><span data-stu-id="7d72e-136">In the previous iteration of the Contact Manager application, we created unit tests to provide a safety net for our code.</span></span> <span data-ttu-id="7d72e-137">建立單元測試的動機是為了讓我們的程式碼更有彈性，來變更。</span><span class="sxs-lookup"><span data-stu-id="7d72e-137">The motivation for creating the unit tests was to make our code more resilient to change.</span></span> <span data-ttu-id="7d72e-138">單元測試，我們可以愉快地保存他們在我們的程式碼中進行任何變更，並立即知道我們是否有中斷現有的功能。</span><span class="sxs-lookup"><span data-stu-id="7d72e-138">With unit tests in place, we can happily make any change to our code and immediately know whether we have broken existing functionality.</span></span>

<span data-ttu-id="7d72e-139">在這個反覆項目，我們可以使用 單元測試完全不同的用途。</span><span class="sxs-lookup"><span data-stu-id="7d72e-139">In this iteration, we use unit tests for an entirely different purpose.</span></span> <span data-ttu-id="7d72e-140">在這個反覆項目，使用單元測試的呼叫應用程式設計原理一部分*測試導向開發*。</span><span class="sxs-lookup"><span data-stu-id="7d72e-140">In this iteration, we use unit tests as part of an application design philosophy called *test-driven development*.</span></span> <span data-ttu-id="7d72e-141">當您練習測試導向開發時，您會先撰寫測試，並再撰寫測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-141">When you practice test-driven development, you write tests first and then write code against the tests.</span></span>

<span data-ttu-id="7d72e-142">更明確地說，當剛好發言測試導向開發，有三個步驟完成建立程式碼時 (紅 / 綠/重構):</span><span class="sxs-lookup"><span data-stu-id="7d72e-142">More precisely, when practicing test-driven development, there are three steps that you complete when creating code (Red/ Green/Refactor):</span></span>

1. <span data-ttu-id="7d72e-143">撰寫單元測試失敗 （紅色）</span><span class="sxs-lookup"><span data-stu-id="7d72e-143">Write a unit test that fails (Red)</span></span>
2. <span data-ttu-id="7d72e-144">撰寫程式碼的單元測試 （綠色）</span><span class="sxs-lookup"><span data-stu-id="7d72e-144">Write code that passes the unit test (Green)</span></span>
3. <span data-ttu-id="7d72e-145">重構程式碼 （重構）</span><span class="sxs-lookup"><span data-stu-id="7d72e-145">Refactor your code (Refactor)</span></span>

<span data-ttu-id="7d72e-146">首先，您會撰寫單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-146">First, you write the unit test.</span></span> <span data-ttu-id="7d72e-147">單元測試應該表達您不需要針對您希望您的程式碼如何運作。</span><span class="sxs-lookup"><span data-stu-id="7d72e-147">The unit test should express your intention for how you expect your code to behave.</span></span> <span data-ttu-id="7d72e-148">當您建立單元測試時，單元測試應該會失敗。</span><span class="sxs-lookup"><span data-stu-id="7d72e-148">When you first create the unit test, the unit test should fail.</span></span> <span data-ttu-id="7d72e-149">測試應該會失敗，因為但是尚未寫入測試任何應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-149">The test should fail because you have not yet written any application code that satisfies the test.</span></span>

<span data-ttu-id="7d72e-150">接下來，您可以撰寫剛好足夠的程式碼單元測試通過的順序。</span><span class="sxs-lookup"><span data-stu-id="7d72e-150">Next, you write just enough code in order for the unit test to pass.</span></span> <span data-ttu-id="7d72e-151">目標是 laziest、 sloppiest 和較快的可能方式撰寫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-151">The goal is to write the code in the laziest, sloppiest and fastest possible way.</span></span> <span data-ttu-id="7d72e-152">您應該不會浪費時間思考應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="7d72e-152">You should not waste time thinking about the architecture of your application.</span></span> <span data-ttu-id="7d72e-153">相反地，您應該專注於撰寫少的程式碼不必滿足單元測試來表示的意圖。</span><span class="sxs-lookup"><span data-stu-id="7d72e-153">Instead, you should focus on writing the minimal amount of code necessary to satisfy the intention expressed by the unit test.</span></span>

<span data-ttu-id="7d72e-154">最後，撰寫程式碼之後，您可以回溯，並考慮您的應用程式的整體架構。</span><span class="sxs-lookup"><span data-stu-id="7d72e-154">Finally, after you have written enough code, you can step back and consider the overall architecture of your application.</span></span> <span data-ttu-id="7d72e-155">在此步驟中，重寫 （重構） 藉由運用軟體設計的程式碼模式-儲存機制模式-例如，讓您的程式碼更容易維護。</span><span class="sxs-lookup"><span data-stu-id="7d72e-155">In this step, you rewrite (refactor) your code by taking advantage of software design patterns -- such as the repository pattern -- so that your code is more maintainable.</span></span> <span data-ttu-id="7d72e-156">因為單元測試所涵蓋的程式碼，您 fearlessly 可以重寫程式碼，在此步驟。</span><span class="sxs-lookup"><span data-stu-id="7d72e-156">You can fearlessly rewrite your code in this step because your code is covered by unit tests.</span></span>

<span data-ttu-id="7d72e-157">有許多優點所導致的練習測試為導向的開發。</span><span class="sxs-lookup"><span data-stu-id="7d72e-157">There are many benefits that result from practicing test-driven development.</span></span> <span data-ttu-id="7d72e-158">第一個步驟，以測試為導向的開發會強迫您專注於實際需要撰寫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-158">First, test-driven development forces you to focus on code that actually needs to be written.</span></span> <span data-ttu-id="7d72e-159">因為您經常會著重於只撰寫程式碼來傳遞特定的測試，您無法從踏入檢討和寫入大量不會使用您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-159">Because you are constantly focused on just writing enough code to pass a particular test, you are prevented from wandering into the weeds and writing massive amounts of code that you will never use.</span></span>

<span data-ttu-id="7d72e-160">第二，「 第一次測試 」 的設計方法也會強迫您撰寫的程式碼的使用方式觀點來從程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-160">Second, a "test first" design methodology forces you to write code from the perspective of how your code will be used.</span></span> <span data-ttu-id="7d72e-161">換句話說，當剛好發言測試導向開發，您要從使用者的觀點不斷地撰寫您的測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-161">In other words, when practicing test-driven development, you are constantly writing your tests from a user perspective.</span></span> <span data-ttu-id="7d72e-162">因此，測試為導向的開發會導致較為簡潔且更容易了解應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="7d72e-162">Therefore, test-driven development can result in cleaner and more understandable APIs.</span></span>

<span data-ttu-id="7d72e-163">最後，測試為導向的開發會強迫您撰寫單元測試做為撰寫的應用程式的一般程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="7d72e-163">Finally, test-driven development forces you to write unit tests as part of the normal process of writing an application.</span></span> <span data-ttu-id="7d72e-164">專案期限接近時，測試是通常超出視窗第一件事。</span><span class="sxs-lookup"><span data-stu-id="7d72e-164">As a project deadline approaches, testing is typically the first thing that goes out the window.</span></span> <span data-ttu-id="7d72e-165">當剛好發言測試導向開發，相反地，您就比較可能是正向撰寫單元測試，因為測試導向開發會讓單元測試中央建置的應用程式的程序的相關。</span><span class="sxs-lookup"><span data-stu-id="7d72e-165">When practicing test-driven development, on the other hand, you are more likely to be virtuous about writing unit tests because test-driven development makes unit tests central to the process of building an application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7d72e-166">若要深入了解測試為導向的應用程式開發，建議您先閱讀 Michael Feathers 書籍**工作有效率地使用舊版程式碼中**。</span><span class="sxs-lookup"><span data-stu-id="7d72e-166">To learn more about test-driven development, I recommend that you read Michael Feathers book **Working Effectively with Legacy Code**.</span></span>


<span data-ttu-id="7d72e-167">在這個反覆項目，我們加入我們的連絡人管理員應用程式的新功能。</span><span class="sxs-lookup"><span data-stu-id="7d72e-167">In this iteration, we add a new feature to our Contact Manager application.</span></span> <span data-ttu-id="7d72e-168">我們新增連絡人群組的支援。</span><span class="sxs-lookup"><span data-stu-id="7d72e-168">We add support for Contact Groups.</span></span> <span data-ttu-id="7d72e-169">您可以使用連絡人群組組織成類別目錄，例如商務連絡人與朋友群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-169">You can use Contact Groups to organize your contacts into categories such as Business and Friend groups.</span></span>

<span data-ttu-id="7d72e-170">我們會遵循的測試為導向的開發程序，將這項新功能新增至我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-170">We'll add this new functionality to our application by following a process of test-driven development.</span></span> <span data-ttu-id="7d72e-171">我們會將第一次寫入我們的單元測試中，我們要撰寫所有程式碼對這些測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-171">We'll write our unit tests first and we'll write all of our code against these tests.</span></span>

## <a name="what-gets-tested"></a><span data-ttu-id="7d72e-172">取得測試的項目</span><span class="sxs-lookup"><span data-stu-id="7d72e-172">What Gets Tested</span></span>

<span data-ttu-id="7d72e-173">如我們所討論的上一個反覆項目，您通常不撰寫資料存取邏輯單元測試或檢視邏輯。</span><span class="sxs-lookup"><span data-stu-id="7d72e-173">As we discussed in the previous iteration, you typically do not write unit tests for data access logic or view logic.</span></span> <span data-ttu-id="7d72e-174">您不 t 寫入單元測試的資料存取邏輯，因為存取資料庫的相對比較慢的作業。</span><span class="sxs-lookup"><span data-stu-id="7d72e-174">You don t write unit tests for data access logic because accessing a database is a relatively slow operation.</span></span> <span data-ttu-id="7d72e-175">您不檢視邏輯 t 寫入單元測試，因為存取檢視需要才能組織好的 web 伺服器不是相對比較慢的作業。</span><span class="sxs-lookup"><span data-stu-id="7d72e-175">You don t write unit tests for view logic because accessing a view requires spinning up a web server which is a relatively slow operation.</span></span> <span data-ttu-id="7d72e-176">除非可以執行測試一再重複速度非常快，您應該不 t 撰寫的單元測試</span><span class="sxs-lookup"><span data-stu-id="7d72e-176">You shouldn t write a unit test unless the test can be executed over and over again very fast</span></span>

<span data-ttu-id="7d72e-177">由於測試為導向的開發由單元測試所驅動的則我們一開始專注於撰寫控制器和商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="7d72e-177">Because test-driven development is driven by unit tests, we focus initially on writing controller and business logic.</span></span> <span data-ttu-id="7d72e-178">我們避免碰觸的資料庫或檢視表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-178">We avoid touching the database or views.</span></span> <span data-ttu-id="7d72e-179">我們贏了 t 修改資料庫或建立本教學課程結束前我們的檢視。</span><span class="sxs-lookup"><span data-stu-id="7d72e-179">We won t modify the database or create our views until the very end of this tutorial.</span></span> <span data-ttu-id="7d72e-180">我們的開頭可以測試的項目。</span><span class="sxs-lookup"><span data-stu-id="7d72e-180">We start with what can be tested.</span></span>

## <a name="creating-user-stories"></a><span data-ttu-id="7d72e-181">建立使用者劇本</span><span class="sxs-lookup"><span data-stu-id="7d72e-181">Creating User Stories</span></span>

<span data-ttu-id="7d72e-182">當剛好發言測試導向開發，您一律從開始撰寫測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-182">When practicing test-driven development, you always start by writing a test.</span></span> <span data-ttu-id="7d72e-183">這會立即引發問題： 如何決定哪個先撰寫測試？</span><span class="sxs-lookup"><span data-stu-id="7d72e-183">This immediately raises the question: How do you decide what test to write first?</span></span> <span data-ttu-id="7d72e-184">若要回答這個問題，您應該撰寫一組[*使用者劇本*](http://en.wikipedia.org/wiki/User_stories)。</span><span class="sxs-lookup"><span data-stu-id="7d72e-184">To answer this question, you should write a set of [*user stories*](http://en.wikipedia.org/wiki/User_stories).</span></span>

<span data-ttu-id="7d72e-185">使用者劇本會是非常簡短的軟體需求 （通常是一個句子） 描述。</span><span class="sxs-lookup"><span data-stu-id="7d72e-185">A user story is a very brief (usually one sentence) description of a software requirement.</span></span> <span data-ttu-id="7d72e-186">其應為非技術性的需求，撰寫從使用者的觀點描述。</span><span class="sxs-lookup"><span data-stu-id="7d72e-186">It should be a non-technical description of a requirement written from a user perspective.</span></span>

<span data-ttu-id="7d72e-187">以下說明新的連絡人群組功能所需的功能的使用者劇本的集：</span><span class="sxs-lookup"><span data-stu-id="7d72e-187">Here s the set of user stories that describe the features required by the new Contact Group functionality:</span></span>

1. <span data-ttu-id="7d72e-188">使用者可以檢視連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-188">User can view a list of contact groups.</span></span>
2. <span data-ttu-id="7d72e-189">使用者可以建立新的連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-189">User can create a new contact group.</span></span>
3. <span data-ttu-id="7d72e-190">使用者可以刪除現有的連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-190">User can delete an existing contact group.</span></span>
4. <span data-ttu-id="7d72e-191">建立新的連絡人時，使用者可以選取連絡人的群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-191">User can select a contact group when creating a new contact.</span></span>
5. <span data-ttu-id="7d72e-192">編輯現有連絡人時，使用者可以選取連絡人的群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-192">User can select a contact group when editing an existing contact.</span></span>
6. <span data-ttu-id="7d72e-193">連絡人群組的清單會顯示在索引檢視中。</span><span class="sxs-lookup"><span data-stu-id="7d72e-193">A list of contact groups is displayed in the Index view.</span></span>
7. <span data-ttu-id="7d72e-194">當使用者按一下連絡人群組時，則會顯示比對的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-194">When a user clicks a contact group, a list of matching contacts is displayed.</span></span>

<span data-ttu-id="7d72e-195">請注意，使用者劇本的這份清單是完全可了解客戶。</span><span class="sxs-lookup"><span data-stu-id="7d72e-195">Notice that this list of user stories is completely understandable by a customer.</span></span> <span data-ttu-id="7d72e-196">沒有未提及的技術實作詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7d72e-196">There is no mention of technical implementation details.</span></span>

<span data-ttu-id="7d72e-197">在程序時建立您的應用程式，使用者劇本的集可能會變得更精簡。</span><span class="sxs-lookup"><span data-stu-id="7d72e-197">While in the process of building your application, the set of user stories can become more refined.</span></span> <span data-ttu-id="7d72e-198">您可能會分成多個劇本 （需求） 的使用者劇本。</span><span class="sxs-lookup"><span data-stu-id="7d72e-198">You might break a user story into multiple stories (requirements).</span></span> <span data-ttu-id="7d72e-199">例如，您可能決定建立新的連絡人群組應該包含驗證。</span><span class="sxs-lookup"><span data-stu-id="7d72e-199">For example, you might decide that creating a new contact group should involve validation.</span></span> <span data-ttu-id="7d72e-200">送出沒有名稱的連絡人群組應該會傳回驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="7d72e-200">Submitting a contact group without a name should return a validation error.</span></span>

<span data-ttu-id="7d72e-201">建立使用者劇本清單之後，您就準備好開始撰寫第一個單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-201">After you create a list of user stories, you are ready to write your first unit test.</span></span> <span data-ttu-id="7d72e-202">我們會先建立單元測試，來檢視連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-202">We'll start by creating a unit test for viewing the list of contact groups.</span></span>

## <a name="listing-contact-groups"></a><span data-ttu-id="7d72e-203">列出連絡人群組</span><span class="sxs-lookup"><span data-stu-id="7d72e-203">Listing Contact Groups</span></span>

<span data-ttu-id="7d72e-204">我們第一個使用者劇本是使用者應該能夠檢視連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-204">Our first user story is that a user should be able to view a list of contact groups.</span></span> <span data-ttu-id="7d72e-205">我們需要 express 此劇本與測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-205">We need to express this story with a test.</span></span>

<span data-ttu-id="7d72e-206">建立新的單元測試，以滑鼠右鍵按一下 [控制器] 資料夾在 ContactManager.Tests 專案中，選取**新增]、 [新增測試**，然後選取**單元測試**範本 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-206">Create a new unit test by right-clicking the Controllers folder in the ContactManager.Tests project, selecting **Add, New Test**, and selecting the **Unit Test** template (see Figure 1).</span></span> <span data-ttu-id="7d72e-207">新的單元測試 GroupControllerTest.vb，再按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7d72e-207">Name the new unit test GroupControllerTest.vb and click the **OK** button.</span></span>


<span data-ttu-id="7d72e-208">[![GroupControllerTest 單元測試](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-208">[![Adding the GroupControllerTest unit test](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)</span></span>

<span data-ttu-id="7d72e-209">**圖 01**: GroupControllerTest 單元測試 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-209">**Figure 01**: Adding the GroupControllerTest unit test([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image2.png))</span></span>


<span data-ttu-id="7d72e-210">第一個單元測試會包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="7d72e-210">Our first unit test is contained in Listing 1.</span></span> <span data-ttu-id="7d72e-211">這項測試會驗證群組控制器的 index （） 方法傳回的一組群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-211">This test verifies that the Index() method of the Group controller returns a set of Groups.</span></span> <span data-ttu-id="7d72e-212">測試會驗證，要資料檢視中傳回群組的集合。</span><span class="sxs-lookup"><span data-stu-id="7d72e-212">The test verifies that a collection of Groups is returned in view data.</span></span>

<span data-ttu-id="7d72e-213">**Listing 1 - Controllers\GroupControllerTest.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-213">**Listing 1 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

<span data-ttu-id="7d72e-214">當您第一次輸入程式碼，在 Visual Studio 中的列表 1 中時，您會取得大量的紅色曲線。</span><span class="sxs-lookup"><span data-stu-id="7d72e-214">When you first type the code in Listing 1 in Visual Studio, you'll get a lot of red squiggly lines.</span></span> <span data-ttu-id="7d72e-215">我們不會建立 GroupController 或群組的類別。</span><span class="sxs-lookup"><span data-stu-id="7d72e-215">We have not created the GroupController or Group classes.</span></span>

<span data-ttu-id="7d72e-216">此時，我們可以 t 甚至組建我們這樣我們就可以 t 的應用程式執行的第一個單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-216">At this point, we can t even build our application so we can t execute our first unit test.</span></span> <span data-ttu-id="7d72e-217">S 良好。</span><span class="sxs-lookup"><span data-stu-id="7d72e-217">That s good.</span></span> <span data-ttu-id="7d72e-218">會算為失敗的測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-218">That counts as a failing test.</span></span> <span data-ttu-id="7d72e-219">因此，我們現在有開始撰寫應用程式程式碼的權限。</span><span class="sxs-lookup"><span data-stu-id="7d72e-219">Therefore, we now have permission to start writing application code.</span></span> <span data-ttu-id="7d72e-220">我們必須撰寫程式碼來執行我們的測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-220">We need to write enough code to execute our test.</span></span>

<span data-ttu-id="7d72e-221">列出 2 中的群組控制器類別包含程式碼將單元測試所需的最低限度。</span><span class="sxs-lookup"><span data-stu-id="7d72e-221">The Group controller class in Listing 2 contains the bare minimum of code required to pass the unit test.</span></span> <span data-ttu-id="7d72e-222">Index 動作傳回群組 （群組類別定義中列出的 3） 以靜態方式自動程式碼的的清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-222">The Index() action returns a statically coded list of Groups (the Group class is defined in Listing 3).</span></span>

<span data-ttu-id="7d72e-223">**Listing 2 - Controllers\GroupController.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-223">**Listing 2 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

<span data-ttu-id="7d72e-224">**Listing 3 - Models\Group.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-224">**Listing 3 - Models\Group.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

<span data-ttu-id="7d72e-225">我們將為 GroupController 和群組類別加入至受測專案之後，第一個單元測試成功完成 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-225">After we add the GroupController and Group classes to our project, our first unit test completes successfully (see Figure 2).</span></span> <span data-ttu-id="7d72e-226">我們已經通過的測試所需的最小工作。</span><span class="sxs-lookup"><span data-stu-id="7d72e-226">We have done the minimum work required to pass the test.</span></span> <span data-ttu-id="7d72e-227">它是慶祝的時間。</span><span class="sxs-lookup"><span data-stu-id="7d72e-227">It is time to celebrate.</span></span>


<span data-ttu-id="7d72e-228">[![Success!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-228">[![Success!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)</span></span>

<span data-ttu-id="7d72e-229">**圖 02**： 成功 ！ ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-229">**Figure 02**: Success!([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image4.png))</span></span>


## <a name="creating-contact-groups"></a><span data-ttu-id="7d72e-230">建立連絡人群組</span><span class="sxs-lookup"><span data-stu-id="7d72e-230">Creating Contact Groups</span></span>

<span data-ttu-id="7d72e-231">現在我們可以移至第二個使用者劇本。</span><span class="sxs-lookup"><span data-stu-id="7d72e-231">Now we can move on to the second user story.</span></span> <span data-ttu-id="7d72e-232">我們需要能夠建立新連絡人的群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-232">We need to be able to create new contact groups.</span></span> <span data-ttu-id="7d72e-233">我們需要這個意圖 express 與測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-233">We need to express this intention with a test.</span></span>

<span data-ttu-id="7d72e-234">列出的 4 中的測試會驗證函式呼叫 create （） 方法與新的群組將群組加入至群組 index （） 方法所傳回的清單。</span><span class="sxs-lookup"><span data-stu-id="7d72e-234">The test in Listing 4 verifies that calling the Create() method with a new Group adds the Group to the list of Groups returned by the Index() method.</span></span> <span data-ttu-id="7d72e-235">換句話說，如果建立新的群組則我應該能夠從 index （） 方法所傳回的群組的清單取得新的群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-235">In other words, if I create a new group then I should be able to get the new Group back from the list of Groups returned by the Index() method.</span></span>

<span data-ttu-id="7d72e-236">**Listing 4 - Controllers\GroupControllerTest.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-236">**Listing 4 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

<span data-ttu-id="7d72e-237">列出的 4 中的測試會呼叫群組控制站與新的連絡人群組 create （） 方法。</span><span class="sxs-lookup"><span data-stu-id="7d72e-237">The test in Listing 4 calls the Group controller Create() method with a new contact Group.</span></span> <span data-ttu-id="7d72e-238">接下來，測試會驗證，群組控制站 index （） 方法的呼叫會傳回新的群組中檢視資料。</span><span class="sxs-lookup"><span data-stu-id="7d72e-238">Next, the test verifies that calling the Group controller Index() method returns the new Group in view data.</span></span>

<span data-ttu-id="7d72e-239">已修改的群組控制站，列出 5 中所包含變更傳遞新的測試所需的最低限度。</span><span class="sxs-lookup"><span data-stu-id="7d72e-239">The modified Group controller in Listing 5 contains the bare minimum of changes required to pass the new test.</span></span>

<span data-ttu-id="7d72e-240">**Listing 5 - Controllers\GroupController.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-240">**Listing 5 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a><span data-ttu-id="7d72e-241">列出 5 中的群組控制站有新的 create （） 動作。</span><span class="sxs-lookup"><span data-stu-id="7d72e-241">The Group controller in Listing 5 has a new Create() action.</span></span> <span data-ttu-id="7d72e-242">這個動作會將群組加入至群組的集合。</span><span class="sxs-lookup"><span data-stu-id="7d72e-242">This action adds a Group to a collection of Groups.</span></span> <span data-ttu-id="7d72e-243">請注意，已修改的 index 動作傳回的群組集合的內容。</span><span class="sxs-lookup"><span data-stu-id="7d72e-243">Notice that the Index() action has been modified to return the contents of the collection of Groups.</span></span>

<span data-ttu-id="7d72e-244">同樣地，我們也可以執行通過的單元測試所需的最低限度。</span><span class="sxs-lookup"><span data-stu-id="7d72e-244">Once again, we have performed the bare minimum amount of work required to pass the unit test.</span></span> <span data-ttu-id="7d72e-245">我們的群組控制站進行這些變更之後，所有我們的單元測試成功。</span><span class="sxs-lookup"><span data-stu-id="7d72e-245">After we make these changes to the Group controller, all of our unit tests pass.</span></span>

## <a name="adding-validation"></a><span data-ttu-id="7d72e-246">新增驗證</span><span class="sxs-lookup"><span data-stu-id="7d72e-246">Adding Validation</span></span>

<span data-ttu-id="7d72e-247">在使用者劇本，已不明確陳述這項需求。</span><span class="sxs-lookup"><span data-stu-id="7d72e-247">This requirement was not stated explicitly in the user story.</span></span> <span data-ttu-id="7d72e-248">不過，它可合理地需要群組有名稱。</span><span class="sxs-lookup"><span data-stu-id="7d72e-248">However, it is reasonable to require that a Group have a name.</span></span> <span data-ttu-id="7d72e-249">否則，連絡人組織成群組將不會非常有用。</span><span class="sxs-lookup"><span data-stu-id="7d72e-249">Otherwise, organizing contacts into Groups would not be very useful.</span></span>

<span data-ttu-id="7d72e-250">列出 6 包含新的測試表達此意圖。</span><span class="sxs-lookup"><span data-stu-id="7d72e-250">Listing 6 contains a new test that expresses this intention.</span></span> <span data-ttu-id="7d72e-251">這項測試會驗證嘗試建立群組，而不需要提供名稱會導致模型狀態中的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7d72e-251">This test verifies that attempting to create a Group without supplying a name results in a validation error message in model state.</span></span>

<span data-ttu-id="7d72e-252">**Listing 6 - Controllers\GroupControllerTest.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-252">**Listing 6 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

<span data-ttu-id="7d72e-253">為了滿足這項測試，我們需要將 Name 屬性新增至群組類別 （請參閱程式碼範例 7）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-253">In order to satisfy this test, we need to add a Name property to our Group class (see Listing 7).</span></span> <span data-ttu-id="7d72e-254">此外，我們需要將稍微不同的驗證邏輯加入至群組 controller s create （） 動作 （請參閱列出 8）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-254">Furthermore, we need to add a tiny bit of validation logic to our Group controller s Create() action (see Listing 8).</span></span>

<span data-ttu-id="7d72e-255">**Listing 7 - Models\Group.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-255">**Listing 7 - Models\Group.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

<span data-ttu-id="7d72e-256">**Listing 8 - Controllers\GroupController.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-256">**Listing 8 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

<span data-ttu-id="7d72e-257">請注意，群組控制站現在 create （） 動作包含驗證和資料庫的邏輯。</span><span class="sxs-lookup"><span data-stu-id="7d72e-257">Notice that the Group controller Create() action now contains both validation and database logic.</span></span> <span data-ttu-id="7d72e-258">目前，群組控制站所使用的資料庫包含記憶體中集合而已。</span><span class="sxs-lookup"><span data-stu-id="7d72e-258">Currently, the database used by the Group controller consists of nothing more than an in-memory collection.</span></span>

## <a name="time-to-refactor"></a><span data-ttu-id="7d72e-259">要重構的時間</span><span class="sxs-lookup"><span data-stu-id="7d72e-259">Time to Refactor</span></span>

<span data-ttu-id="7d72e-260">第三步中紅/綠/重構是重構部分。</span><span class="sxs-lookup"><span data-stu-id="7d72e-260">The third step in Red/Green/Refactor is the Refactor part.</span></span> <span data-ttu-id="7d72e-261">此時，我們需要的程式碼，並考慮我們可以重構我們改善其設計的應用程式的方式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-261">At this point, we need to step back from our code and consider how we can refactor our application to improve its design.</span></span> <span data-ttu-id="7d72e-262">重構階段是在我們認為硬碟的最佳方式，實作軟體設計的原則和模式的階段。</span><span class="sxs-lookup"><span data-stu-id="7d72e-262">The Refactor stage is the stage at which we think hard about the best way of implementing software design principles and patterns.</span></span>

<span data-ttu-id="7d72e-263">我們可以自由修改任何方式來改善程式碼的設計，我們選擇的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-263">We are free to modify our code in any way that we choose to improve the design of the code.</span></span> <span data-ttu-id="7d72e-264">我們有防止我們中斷現有功能的單元測試的安全網。</span><span class="sxs-lookup"><span data-stu-id="7d72e-264">We have a safety net of unit tests that prevent us from breaking existing functionality.</span></span>

<span data-ttu-id="7d72e-265">現在，我們的群組控制站是好的軟體設計的觀點混亂。</span><span class="sxs-lookup"><span data-stu-id="7d72e-265">Right now, our Group controller is a mess from the perspective of good software design.</span></span> <span data-ttu-id="7d72e-266">群組控制站包含盤的混亂的驗證和資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-266">The Group controller contains a tangled mess of validation and data access code.</span></span> <span data-ttu-id="7d72e-267">若要避免違反單一責任原則，我們需要將這些考量分成不同類別。</span><span class="sxs-lookup"><span data-stu-id="7d72e-267">To avoid violating the Single Responsibility Principle, we need to separate these concerns into different classes.</span></span>

<span data-ttu-id="7d72e-268">我們已重構的群組控制器類別包含在列出 9。</span><span class="sxs-lookup"><span data-stu-id="7d72e-268">Our refactored Group controller class is contained in Listing 9.</span></span> <span data-ttu-id="7d72e-269">控制器已修改成使用 ContactManager 服務層。</span><span class="sxs-lookup"><span data-stu-id="7d72e-269">The controller has been modified to use the ContactManager service layer.</span></span> <span data-ttu-id="7d72e-270">這是我們與連絡人控制器所使用的相同服務層。</span><span class="sxs-lookup"><span data-stu-id="7d72e-270">This is the same service layer that we use with the Contact controller.</span></span>

<span data-ttu-id="7d72e-271">列出 10 包含新方法加入至 ContactManager 服務層，以支援驗證、 列出和建立群組。</span><span class="sxs-lookup"><span data-stu-id="7d72e-271">Listing 10 contains the new methods added to the ContactManager service layer to support validating, listing, and creating groups.</span></span> <span data-ttu-id="7d72e-272">IContactManagerService 介面已更新以包含新的方法。</span><span class="sxs-lookup"><span data-stu-id="7d72e-272">The IContactManagerService interface was updated to include the new methods.</span></span>

<span data-ttu-id="7d72e-273">列出 11 包含新的 FakeContactManagerRepository 類別可實作 IContactManagerRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="7d72e-273">Listing 11 contains a new FakeContactManagerRepository class that implements the IContactManagerRepository interface.</span></span> <span data-ttu-id="7d72e-274">不同於 EntityContactManagerRepository 類別也實作 IContactManagerRepository 介面，新 FakeContactManagerRepository 類別不會不會與資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="7d72e-274">Unlike the EntityContactManagerRepository class that also implements the IContactManagerRepository interface, our new FakeContactManagerRepository class does not communicate with the database.</span></span> <span data-ttu-id="7d72e-275">FakeContactManagerRepository 類別會做為 proxy 的記憶體中集合使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="7d72e-275">The FakeContactManagerRepository class uses an in-memory collection as a proxy for the database.</span></span> <span data-ttu-id="7d72e-276">我們將在我們的單元測試中使用這個類別，像是假的儲存機制的層級。</span><span class="sxs-lookup"><span data-stu-id="7d72e-276">We'll use this class in our unit tests as a fake repository layer.</span></span>

<span data-ttu-id="7d72e-277">**Listing 9 - Controllers\GroupController.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-277">**Listing 9 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

<span data-ttu-id="7d72e-278">**Listing 10 - Controllers\ContactManagerService.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-278">**Listing 10 - Controllers\ContactManagerService.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

<span data-ttu-id="7d72e-279">**Listing 11 - Controllers\FakeContactManagerRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-279">**Listing 11 - Controllers\FakeContactManagerRepository.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


<span data-ttu-id="7d72e-280">修改 IContactManagerRepository 介面需要使用在 EntityContactManagerRepository 類別中實作的 CreateGroup() 和 ListGroups() 方法。</span><span class="sxs-lookup"><span data-stu-id="7d72e-280">Modifying the IContactManagerRepository interface requires use to implement the CreateGroup() and ListGroups() methods in the EntityContactManagerRepository class.</span></span> <span data-ttu-id="7d72e-281">Laziest 且最快的方法是新增虛設常式方法，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="7d72e-281">The laziest and fastest way to do this is to add stub methods that look like this:</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


<span data-ttu-id="7d72e-282">最後，這些變更，我們的應用程式的設計會要求我們對我們的單元測試中進行一些修改。</span><span class="sxs-lookup"><span data-stu-id="7d72e-282">Finally, these changes to the design of our application require us to make some modifications to our unit tests.</span></span> <span data-ttu-id="7d72e-283">我們現在需要執行單元測試時使用 FakeContactManagerRepository。</span><span class="sxs-lookup"><span data-stu-id="7d72e-283">We now need to use the FakeContactManagerRepository when performing the unit tests.</span></span> <span data-ttu-id="7d72e-284">更新的 GroupControllerTest 類別被包含在列出 12。</span><span class="sxs-lookup"><span data-stu-id="7d72e-284">The updated GroupControllerTest class is contained in Listing 12.</span></span>

<span data-ttu-id="7d72e-285">**Listing 12 - Controllers\GroupControllerTest.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-285">**Listing 12 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

<span data-ttu-id="7d72e-286">我們進行所有這些之後，變更同樣地，所有我們單元測試成功。</span><span class="sxs-lookup"><span data-stu-id="7d72e-286">After we make all of these changes, once again, all of our unit tests pass.</span></span> <span data-ttu-id="7d72e-287">我們已經完成紅/綠/重構的整個週期。</span><span class="sxs-lookup"><span data-stu-id="7d72e-287">We have completed the entire cycle of Red/Green/Refactor.</span></span> <span data-ttu-id="7d72e-288">我們已經實作的第一個的兩個使用者劇本。</span><span class="sxs-lookup"><span data-stu-id="7d72e-288">We have implemented the first two user stories.</span></span> <span data-ttu-id="7d72e-289">我們現在有支援單元測試中的使用者劇本所表示的需求。</span><span class="sxs-lookup"><span data-stu-id="7d72e-289">We now have supporting unit tests for the requirements expressed in the user stories.</span></span> <span data-ttu-id="7d72e-290">實作使用者劇本的其餘部分，必須重複相同的循環的 紅/綠/重構。</span><span class="sxs-lookup"><span data-stu-id="7d72e-290">Implementing the remainder of the user stories involves repeating the same cycle of Red/Green/Refactor.</span></span>

## <a name="modifying-our-database"></a><span data-ttu-id="7d72e-291">修改資料庫</span><span class="sxs-lookup"><span data-stu-id="7d72e-291">Modifying our Database</span></span>

<span data-ttu-id="7d72e-292">不幸的是，即使我們已符合所有表示我們的單元測試的需求，我們是未完成的工作。</span><span class="sxs-lookup"><span data-stu-id="7d72e-292">Unfortunately, even though we have satisfied all of the requirements expressed by our unit tests, our work is not done.</span></span> <span data-ttu-id="7d72e-293">我們仍需要修改資料庫。</span><span class="sxs-lookup"><span data-stu-id="7d72e-293">We still need to modify our database.</span></span>

<span data-ttu-id="7d72e-294">我們需要建立新群組的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-294">We need to create a new Group database table.</span></span> <span data-ttu-id="7d72e-295">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7d72e-295">Follow these steps:</span></span>

1. <span data-ttu-id="7d72e-296">在 [伺服器總管] 視窗中，以滑鼠右鍵按一下 [資料表] 資料夾並選取功能表選項**加入新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="7d72e-296">In the Server Explorer window, right-click the Tables folder and select the menu option **Add New Table**.</span></span>
2. <span data-ttu-id="7d72e-297">輸入 資料表設計工具，如下所述的兩個資料行。</span><span class="sxs-lookup"><span data-stu-id="7d72e-297">Enter the two columns described below in the Table Designer.</span></span>
3. <span data-ttu-id="7d72e-298">識別碼資料行標示為 primary key 和識別資料行。</span><span class="sxs-lookup"><span data-stu-id="7d72e-298">Mark the Id column as a primary key and Identity column.</span></span>
4. <span data-ttu-id="7d72e-299">磁碟的圖示，即可儲存群組命名新的資料表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-299">Save the new table with the name Groups by clicking the icon of the floppy.</span></span>

<a id="0.12_table01"></a>


| <span data-ttu-id="7d72e-300">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="7d72e-300">**Column Name**</span></span> | <span data-ttu-id="7d72e-301">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="7d72e-301">**Data Type**</span></span> | <span data-ttu-id="7d72e-302">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="7d72e-302">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7d72e-303">ID</span><span class="sxs-lookup"><span data-stu-id="7d72e-303">Id</span></span> | <span data-ttu-id="7d72e-304">int</span><span class="sxs-lookup"><span data-stu-id="7d72e-304">int</span></span> | <span data-ttu-id="7d72e-305">False</span><span class="sxs-lookup"><span data-stu-id="7d72e-305">False</span></span> |
| <span data-ttu-id="7d72e-306">名稱</span><span class="sxs-lookup"><span data-stu-id="7d72e-306">Name</span></span> | <span data-ttu-id="7d72e-307">Nvarchar （50)</span><span class="sxs-lookup"><span data-stu-id="7d72e-307">nvarchar(50)</span></span> | <span data-ttu-id="7d72e-308">False</span><span class="sxs-lookup"><span data-stu-id="7d72e-308">False</span></span> |


<span data-ttu-id="7d72e-309">接下來，我們需要從 [連絡人] 資料表中刪除所有的資料 (否則我們贏了將無法建立連絡人和群組的資料表之間的關聯性)。</span><span class="sxs-lookup"><span data-stu-id="7d72e-309">Next, we need to delete all of the data from the Contacts table (otherwise, we won t be able to create a relationship between the Contacts and Groups tables).</span></span> <span data-ttu-id="7d72e-310">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7d72e-310">Follow these steps:</span></span>

1. <span data-ttu-id="7d72e-311">以滑鼠右鍵按一下 [連絡人] 資料表，然後選取功能表選項**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="7d72e-311">Right-click the Contacts table and select the menu option **Show Table Data**.</span></span>
2. <span data-ttu-id="7d72e-312">刪除所有資料列。</span><span class="sxs-lookup"><span data-stu-id="7d72e-312">Delete all of the rows.</span></span>

<span data-ttu-id="7d72e-313">接下來，我們必須定義在群組的資料庫資料表和現有的連絡人資料庫資料表之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="7d72e-313">Next, we need to define a relationship between the Groups database table and the existing Contacts database table.</span></span> <span data-ttu-id="7d72e-314">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7d72e-314">Follow these steps:</span></span>

1. <span data-ttu-id="7d72e-315">按兩下以開啟 資料表設計工具的 伺服器總管 視窗中的 連絡人 資料表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-315">Double-click the Contacts table in the Server Explorer window to open the Table Designer.</span></span>
2. <span data-ttu-id="7d72e-316">將新的整數資料行加入至 [連絡人] 資料表名為 GroupId。</span><span class="sxs-lookup"><span data-stu-id="7d72e-316">Add a new integer column to the Contacts table named GroupId.</span></span>
3. <span data-ttu-id="7d72e-317">按一下 [關聯] 按鈕以開啟外部索引鍵關聯性對話方塊 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-317">Click the Relationship button to open the Foreign Key Relationships dialog (see Figure 3).</span></span>
4. <span data-ttu-id="7d72e-318">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7d72e-318">Click the Add button.</span></span>
5. <span data-ttu-id="7d72e-319">按一下 [顯示資料表和資料行規格] 按鈕旁邊的省略符號按鈕。</span><span class="sxs-lookup"><span data-stu-id="7d72e-319">Click the ellipsis button that appears next to the Table and Columns Specification button.</span></span>
6. <span data-ttu-id="7d72e-320">在 資料表和資料行 對話方塊中，選取 群組做為的主索引鍵資料表和主索引鍵資料行的識別碼。</span><span class="sxs-lookup"><span data-stu-id="7d72e-320">In the Tables and Columns dialog, select Groups as the primary key table and Id as the primary key column.</span></span> <span data-ttu-id="7d72e-321">選取連絡人做為外部索引鍵資料表和外部索引鍵資料行的識別碼 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-321">Select Contacts as the foreign key table and GroupId as the foreign key column (see Figure 4).</span></span> <span data-ttu-id="7d72e-322">按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7d72e-322">Click the OK button.</span></span>
7. <span data-ttu-id="7d72e-323">在下**INSERT 及 UPDATE 規格**，選取值**Cascade**如**刪除規則**。</span><span class="sxs-lookup"><span data-stu-id="7d72e-323">Under **INSERT and UPDATE Specification**, select the value **Cascade** for **Delete Rule**.</span></span>
8. <span data-ttu-id="7d72e-324">按一下 [關閉] 按鈕以關閉 [外部索引鍵關聯性] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7d72e-324">Click the Close button to close the Foreign Key Relationships dialog.</span></span>
9. <span data-ttu-id="7d72e-325">按一下 [儲存] 按鈕將變更儲存到 [連絡人] 資料表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-325">Click the Save button to save the changes to the Contacts table.</span></span>


<span data-ttu-id="7d72e-326">[![建立資料庫資料表關聯性](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-326">[![Creating a database table relationship](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)</span></span>

<span data-ttu-id="7d72e-327">**圖 03**： 建立資料庫資料表關聯性 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-327">**Figure 03**: Creating a database table relationship ([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image6.png))</span></span>


<span data-ttu-id="7d72e-328">[![指定資料表關聯性](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-328">[![Specifying table relationships](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)</span></span>

<span data-ttu-id="7d72e-329">**圖 04**： 指定資料表關聯性 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-329">**Figure 04**: Specifying table relationships([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image8.png))</span></span>


### <a name="updating-our-data-model"></a><span data-ttu-id="7d72e-330">更新我們的資料模型</span><span class="sxs-lookup"><span data-stu-id="7d72e-330">Updating our Data Model</span></span>

<span data-ttu-id="7d72e-331">接下來，我們需要更新我們的資料模型，以代表新的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-331">Next, we need to update our data model to represent the new database table.</span></span> <span data-ttu-id="7d72e-332">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7d72e-332">Follow these steps:</span></span>

1. <span data-ttu-id="7d72e-333">按兩下 ContactManagerModel.edmx 檔案以開啟 Entity Designer 的 模型 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7d72e-333">Double-click the ContactManagerModel.edmx file in the Models folder to open the Entity Designer.</span></span>
2. <span data-ttu-id="7d72e-334">在設計工具介面上按一下滑鼠右鍵，然後選取功能表選項**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="7d72e-334">Right-click the Designer surface and select the menu option **Update Model from Database**.</span></span>
3. <span data-ttu-id="7d72e-335">在更新精靈中，選取群組的資料表，並按一下 [完成] 按鈕 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-335">In the Update Wizard, select the Groups table and click the Finish button (see Figure 5).</span></span>
4. <span data-ttu-id="7d72e-336">以滑鼠右鍵按一下群組實體，然後選取功能表選項**重新命名**。</span><span class="sxs-lookup"><span data-stu-id="7d72e-336">Right-click the Groups entity and select the menu option **Rename**.</span></span> <span data-ttu-id="7d72e-337">變更名稱*群組*實體*群組*（單數）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-337">Change the name of the *Groups* entity to *Group* (singular).</span></span>
5. <span data-ttu-id="7d72e-338">以滑鼠右鍵按一下連絡人實體下方會顯示群組導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="7d72e-338">Right-click the Groups navigation property that appears at the bottom of the Contact entity.</span></span> <span data-ttu-id="7d72e-339">變更名稱*群組*導覽屬性來*群組*（單數）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-339">Change the name of the *Groups* navigation property to *Group* (singular).</span></span>


<span data-ttu-id="7d72e-340">[![更新資料庫中的 Entity Framework 模型](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-340">[![Updating an Entity Framework model from the database](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)</span></span>

<span data-ttu-id="7d72e-341">**圖 05**： 更新資料庫中的 Entity Framework 模型 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-341">**Figure 05**: Updating an Entity Framework model from the database([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image10.png))</span></span>


<span data-ttu-id="7d72e-342">完成這些步驟之後，您的資料模型將代表連絡人和群組的資料表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-342">After you complete these steps, your data model will represent both the Contacts and Groups tables.</span></span> <span data-ttu-id="7d72e-343">實體設計工具應該會顯示這兩個實體 （請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="7d72e-343">The Entity Designer should show both entities (see Figure 6).</span></span>


<span data-ttu-id="7d72e-344">[![顯示群組和連絡人的實體設計工具](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-344">[![Entity Designer displaying Group and Contact](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)</span></span>

<span data-ttu-id="7d72e-345">**圖 06**： 顯示群組和連絡人的實體設計工具 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-345">**Figure 06**: Entity Designer displaying Group and Contact([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image12.png))</span></span>


### <a name="creating-our-repository-classes"></a><span data-ttu-id="7d72e-346">建立儲存機制類別</span><span class="sxs-lookup"><span data-stu-id="7d72e-346">Creating our Repository Classes</span></span>

<span data-ttu-id="7d72e-347">接下來，我們需要實作儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="7d72e-347">Next, we need to implement our repository class.</span></span> <span data-ttu-id="7d72e-348">進行這個反覆項目期間，我們加入數個新方法 IContactManagerRepository 介面撰寫程式碼，以符合我們的單元測試時。</span><span class="sxs-lookup"><span data-stu-id="7d72e-348">Over the course of this iteration, we added several new methods to the IContactManagerRepository interface while writing code to satisfy our unit tests.</span></span> <span data-ttu-id="7d72e-349">IContactManagerRepository 介面的最終版本會包含在程式碼範例 14。</span><span class="sxs-lookup"><span data-stu-id="7d72e-349">The final version of the IContactManagerRepository interface is contained in Listing 14.</span></span>

<span data-ttu-id="7d72e-350">**列出 14-Models\IContactManagerRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-350">**Listing 14 - Models\IContactManagerRepository.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

<span data-ttu-id="7d72e-351">我們樂園 t 實際實作任何有關連絡人群組，我們實際 EntityContactManagerRepository 類別中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="7d72e-351">We haven t actually implemented any of the methods related to working with contact groups in our real EntityContactManagerRepository class.</span></span> <span data-ttu-id="7d72e-352">目前，EntityContactManagerRepository 類別具有虛設常式方法每 IContactManagerRepository 介面中所列的連絡人群組方法。</span><span class="sxs-lookup"><span data-stu-id="7d72e-352">Currently, the EntityContactManagerRepository class has stub methods for each of the contact group methods listed in the IContactManagerRepository interface.</span></span> <span data-ttu-id="7d72e-353">例如，ListGroups() 方法目前看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="7d72e-353">For example, the ListGroups() method currently looks like this:</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

<span data-ttu-id="7d72e-354">虛設常式方法會啟用我們編譯我們的應用程式，並傳遞單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-354">The stub methods enabled us to compile our application and pass the unit tests.</span></span> <span data-ttu-id="7d72e-355">不過，現在它是以實際實作這些方法的時間。</span><span class="sxs-lookup"><span data-stu-id="7d72e-355">However, now it is time to actually implement these methods.</span></span> <span data-ttu-id="7d72e-356">EntityContactManagerRepository 類別的最終版本會包含在列出 13。</span><span class="sxs-lookup"><span data-stu-id="7d72e-356">The final version of the EntityContactManagerRepository class is contained in Listing 13.</span></span>

<span data-ttu-id="7d72e-357">**Listing 13 - Models\EntityContactManagerRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="7d72e-357">**Listing 13 - Models\EntityContactManagerRepository.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a><span data-ttu-id="7d72e-358">建立檢視</span><span class="sxs-lookup"><span data-stu-id="7d72e-358">Creating the Views</span></span>

<span data-ttu-id="7d72e-359">當您使用預設 ASP.NET 檢視引擎 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-359">ASP.NET MVC application when you use the default ASP.NET view engine.</span></span> <span data-ttu-id="7d72e-360">因此，您不要建立檢視以回應特定的單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d72e-360">So, you don t create views in response to a particular unit test.</span></span> <span data-ttu-id="7d72e-361">不過，因為應用程式將不具有檢視毫無用處，我們可以 t 完成這個反覆項目，而不需要建立和修改連絡人管理員應用程式中包含的檢視。</span><span class="sxs-lookup"><span data-stu-id="7d72e-361">However, because an application would be useless without views, we can t complete this iteration without creating and modifying the views contained in the Contact Manager application.</span></span>

<span data-ttu-id="7d72e-362">我們需要建立下列新的檢視來管理連絡人群組 （請參閱圖 7）：</span><span class="sxs-lookup"><span data-stu-id="7d72e-362">We need to create the following new views for managing contact groups (see Figure 7):</span></span>

- <span data-ttu-id="7d72e-363">Views\Group\Index.aspx-連絡人群組的顯示清單</span><span class="sxs-lookup"><span data-stu-id="7d72e-363">Views\Group\Index.aspx - Displays list of contact groups</span></span>
- <span data-ttu-id="7d72e-364">Views\Group\Delete.aspx-刪除連絡人群組的顯示確認表單</span><span class="sxs-lookup"><span data-stu-id="7d72e-364">Views\Group\Delete.aspx - Displays confirmation form for deleting a contact group</span></span>


<span data-ttu-id="7d72e-365">[![群組索引檢視](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-365">[![The Group Index view](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)</span></span>

<span data-ttu-id="7d72e-366">**圖 07**: 群組索引檢視 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-366">**Figure 07**: The Group Index view([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image14.png))</span></span>


<span data-ttu-id="7d72e-367">我們需要修改下列現有檢視，讓它們包含連絡人群組：</span><span class="sxs-lookup"><span data-stu-id="7d72e-367">We need to modify the following existing views so that they include contact groups:</span></span>

- <span data-ttu-id="7d72e-368">Views\Home\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="7d72e-368">Views\Home\Create.aspx</span></span>
- <span data-ttu-id="7d72e-369">Views\Home\Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="7d72e-369">Views\Home\Edit.aspx</span></span>
- <span data-ttu-id="7d72e-370">Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="7d72e-370">Views\Home\Index.aspx</span></span>

<span data-ttu-id="7d72e-371">您可以藉由查看此教學課程隨附的 Visual Studio 應用程式查看修改過的檢視。</span><span class="sxs-lookup"><span data-stu-id="7d72e-371">You can see the modified views by looking at the Visual Studio application that accompanies this tutorial.</span></span> <span data-ttu-id="7d72e-372">例如，圖 8 說明連絡人索引檢視。</span><span class="sxs-lookup"><span data-stu-id="7d72e-372">For example, Figure 8 illustrates the Contact Index view.</span></span>


<span data-ttu-id="7d72e-373">[![連絡人索引檢視](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="7d72e-373">[![The Contact Index view](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)</span></span>

<span data-ttu-id="7d72e-374">**圖 08**: 連絡人索引檢視 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="7d72e-374">**Figure 08**: The Contact Index view([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image16.png))</span></span>


## <a name="summary"></a><span data-ttu-id="7d72e-375">總結</span><span class="sxs-lookup"><span data-stu-id="7d72e-375">Summary</span></span>

<span data-ttu-id="7d72e-376">在這個反覆項目，我們加入的新功能為連絡人管理員應用程式遵循測試為導向的開發應用程式的設計方法。</span><span class="sxs-lookup"><span data-stu-id="7d72e-376">In this iteration, we added new functionality to our Contact Manager application by following a test-driven development application design methodology.</span></span> <span data-ttu-id="7d72e-377">我們一開始會建立一組使用者劇本。</span><span class="sxs-lookup"><span data-stu-id="7d72e-377">We started by creating a set of user stories.</span></span> <span data-ttu-id="7d72e-378">我們建立單元測試集對應至使用者劇本所表示的需求。</span><span class="sxs-lookup"><span data-stu-id="7d72e-378">We created a set of unit tests that corresponds to the requirements expressed by the user stories.</span></span> <span data-ttu-id="7d72e-379">最後，我們會撰寫剛好足夠的程式碼，來滿足表示單元測試的需求。</span><span class="sxs-lookup"><span data-stu-id="7d72e-379">Finally, we wrote just enough code to satisfy the requirements expressed by the unit tests.</span></span>

<span data-ttu-id="7d72e-380">我們已完成寫入程式碼來滿足表示單元測試的需求之後，我們已更新我們的資料庫和檢視表。</span><span class="sxs-lookup"><span data-stu-id="7d72e-380">After we finished writing enough code to satisfy the requirements expressed by the unit tests, we updated our database and views.</span></span> <span data-ttu-id="7d72e-381">我們加入到資料庫的新群組的資料表，並更新我們 Entity Framework 資料模型。</span><span class="sxs-lookup"><span data-stu-id="7d72e-381">We added a new Groups table to our database and updated our Entity Framework Data Model.</span></span> <span data-ttu-id="7d72e-382">我們也會建立和修改一組檢視。</span><span class="sxs-lookup"><span data-stu-id="7d72e-382">We also created and modified a set of views.</span></span>

<span data-ttu-id="7d72e-383">在下一個反覆項目-最後一個反覆項目-我們重新撰寫以善用 Ajax 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d72e-383">In the next iteration -- the final iteration -- we rewrite our application to take advantage of Ajax.</span></span> <span data-ttu-id="7d72e-384">利用 Ajax，我們將會改善回應性和連絡人管理員應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="7d72e-384">By taking advantage of Ajax, we'll improve the responsiveness and performance of the Contact Manager application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d72e-385">[上一頁](iteration-5-create-unit-tests-vb.md)
> [下一頁](iteration-7-add-ajax-functionality-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7d72e-385">[Previous](iteration-5-create-unit-tests-vb.md)
[Next](iteration-7-add-ajax-functionality-vb.md)</span></span>
