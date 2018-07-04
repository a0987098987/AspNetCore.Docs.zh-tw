---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: '反覆項目 #7 – 新增 Ajax 功能 (VB) |Microsoft Docs'
author: microsoft
description: 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: f951135014c46ef2685955700dc5649581539989
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394251"
---
<a name="iteration-7--add-ajax-functionality-vb"></a><span data-ttu-id="d9aa6-103">反覆項目 #7 – 新增 Ajax 功能 (VB)</span><span class="sxs-lookup"><span data-stu-id="d9aa6-103">Iteration #7 – Add Ajax functionality (VB)</span></span>
====================
<span data-ttu-id="d9aa6-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d9aa6-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d9aa6-105">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="d9aa6-105">Download Code</span></span>](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> <span data-ttu-id="d9aa6-106">在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-106">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="d9aa6-107">建立連絡人管理 ASP.NET MVC 應用程式 (VB)</span><span class="sxs-lookup"><span data-stu-id="d9aa6-107">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="d9aa6-108">在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-108">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="d9aa6-109">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-109">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="d9aa6-110">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-110">We build the application over multiple iterations.</span></span> <span data-ttu-id="d9aa6-111">每次反覆運算時，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-111">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="d9aa6-112">此多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-112">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="d9aa6-113">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-113">Iteration #1 - Create the application.</span></span> <span data-ttu-id="d9aa6-114">在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-114">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="d9aa6-115">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-115">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="d9aa6-116">反覆項目 #2-讓應用程式看起來不錯。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-116">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="d9aa6-117">這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-117">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="d9aa6-118">反覆項目 #3-新增表單驗證。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-118">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="d9aa6-119">在第三個反覆項目，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-119">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="d9aa6-120">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-120">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="d9aa6-121">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-121">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="d9aa6-122">反覆項目 #4-進行鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-122">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="d9aa6-123">在此第三個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-123">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="d9aa6-124">比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-124">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="d9aa6-125">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-125">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="d9aa6-126">在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-126">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="d9aa6-127">我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-127">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="d9aa6-128">反覆項目 #6-使用測試導向開發。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-128">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="d9aa6-129">在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-129">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="d9aa6-130">在這個反覆項目，我們會新增連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-130">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="d9aa6-131">反覆項目 #7-新增 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-131">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="d9aa6-132">在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-132">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="d9aa6-133">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="d9aa6-133">This Iteration</span></span>

<span data-ttu-id="d9aa6-134">在 Contact Manager 應用程式的這個反覆項目，我們可以重構我們的應用程式，才能使用 Ajax。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-134">In this iteration of the Contact Manager application, we refactor our application to make use of Ajax.</span></span> <span data-ttu-id="d9aa6-135">利用 Ajax，我們讓我們的應用程式回應速度更快。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-135">By taking advantage of Ajax, we make our application more responsive.</span></span> <span data-ttu-id="d9aa6-136">我們可以避免當我們需要更新只有特定區域網頁中的呈現整個頁面。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-136">We can avoid rendering an entire page when we need to update only a certain region in a page.</span></span>

<span data-ttu-id="d9aa6-137">我們將重構我們 [索引] 檢視，讓我們不 t 不必重新顯示整個頁面，只要有人選取新的連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-137">We'll refactor our Index view so that we don t need to redisplay the entire page whenever someone selects a new contact group.</span></span> <span data-ttu-id="d9aa6-138">相反地，當有人按一下連絡人群組，我們將只更新的連絡人清單，然後將其餘頁面的單獨。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-138">Instead, when someone clicks a contact group, we'll just update the list of contacts and leave the rest of the page alone.</span></span>

<span data-ttu-id="d9aa6-139">我們也會變更我們刪除連結運作的方式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-139">We'll also change the way our delete link works.</span></span> <span data-ttu-id="d9aa6-140">不會顯示個別的確認頁面，我們會顯示 JavaScript 確認對話方塊。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-140">Instead of displaying a separate confirmation page, we'll display a JavaScript confirmation dialog.</span></span> <span data-ttu-id="d9aa6-141">如果您確認您想要刪除的連絡人，針對要從資料庫刪除的連絡人記錄之伺服器執行 HTTP DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-141">If you confirm that you want to delete a contact, an HTTP DELETE operation is performed against the server to delete the contact record from the database.</span></span>

<span data-ttu-id="d9aa6-142">此外，我們也會利用 jQuery 來加入我們的 [索引] 檢視中的動畫效果。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-142">Furthermore, we will take advantage of jQuery to add animation effects to our Index view.</span></span> <span data-ttu-id="d9aa6-143">正在從伺服器擷取新的連絡人清單時，我們會顯示動畫。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-143">We'll display an animation when the new list of contacts is being fetched from the server.</span></span>

<span data-ttu-id="d9aa6-144">最後，我們也會利用 ASP.NET AJAX 架構支援管理瀏覽器歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-144">Finally, we'll take advantage of the ASP.NET AJAX framework support for managing browser history.</span></span> <span data-ttu-id="d9aa6-145">每當我們執行更新的連絡人清單的 Ajax 呼叫時，我們將建立記錄點。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-145">We'll create history points whenever we perform an Ajax call to update the contact list.</span></span> <span data-ttu-id="d9aa6-146">如此一來，瀏覽器向後和向前按鈕將會運作。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-146">That way, the browser backward and forward buttons will work.</span></span>

## <a name="why-use-ajax"></a><span data-ttu-id="d9aa6-147">為何要使用 Ajax？</span><span class="sxs-lookup"><span data-stu-id="d9aa6-147">Why use Ajax?</span></span>

<span data-ttu-id="d9aa6-148">使用 Ajax，有許多優點。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-148">Using Ajax has many benefits.</span></span> <span data-ttu-id="d9aa6-149">首先，將 Ajax 功能新增到應用程式會導致較佳使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-149">First, adding Ajax functionality to an application results in a better user experience.</span></span> <span data-ttu-id="d9aa6-150">在一般的 web 應用程式中，整個頁面必須回傳至伺服器，每次使用者執行的動作。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-150">In a normal web application, the entire page must be posted back to the server each and every time a user performs an action.</span></span> <span data-ttu-id="d9aa6-151">每當您執行的動作時，瀏覽器鎖定和的使用者必須等到擷取和重新顯示整個頁面。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-151">Whenever you perform an action, the browser locks and the user must wait until the entire page is fetched and redisplayed.</span></span>

<span data-ttu-id="d9aa6-152">這是在桌面應用程式的情況下，無法接受的體驗。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-152">This would be an unacceptable experience in the case of a desktop application.</span></span> <span data-ttu-id="d9aa6-153">不過，傳統上，我們有實質這個不正確的使用者體驗，在 web 應用程式因為我們不知道我們可以執行任何高愈好。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-153">But, traditionally, we lived with this bad user experience in the case of a web application because we did not know that we could do any better.</span></span> <span data-ttu-id="d9aa6-154">我們認為它是 web 應用程式的限制時，實際上，它只是我們 imaginations 的限制。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-154">We thought it was a limitation of web applications when, in actuality, it was just a limitation of our imaginations.</span></span>

<span data-ttu-id="d9aa6-155">Ajax 應用程式中，您不會導致中止只為了更新頁面的使用者體驗的 t 需要。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-155">In an Ajax application, you don t need to bring the user experience to a halt just to update a page.</span></span> <span data-ttu-id="d9aa6-156">相反地，您也可以更新該頁面背景中執行的非同步要求。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-156">Instead, you can perform an asynchronous request in the background to update the page.</span></span> <span data-ttu-id="d9aa6-157">您不 t 強制使用者等候取得更新的網頁部分時。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-157">You don t force the user to wait while part of the page gets updated.</span></span>

<span data-ttu-id="d9aa6-158">利用 Ajax，您也可以改善您的應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-158">By taking advantage of Ajax, you also can improve the performance of your application.</span></span> <span data-ttu-id="d9aa6-159">請考慮連絡管理員應用程式的運作方式現在不使用 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-159">Consider how the Contact Manager application works right now without Ajax functionality.</span></span> <span data-ttu-id="d9aa6-160">當您按一下連絡人群組時，就必須重新顯示整個索引檢視。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-160">When you click a contact group, the entire Index view must be redisplayed.</span></span> <span data-ttu-id="d9aa6-161">必須從資料庫伺服器擷取的連絡人清單和連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-161">The list of contacts and list of contact groups must be retrieved from the database server.</span></span> <span data-ttu-id="d9aa6-162">所有這些資料必須傳遞透過網路從 web 伺服器到網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-162">All of this data must be passed across the wire from web server to web browser.</span></span>

<span data-ttu-id="d9aa6-163">我們將 Ajax 功能新增至我們的應用程式之後，不過，我們就可以避免當使用者按一下連絡人群組，請選擇整個頁面。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-163">After we add Ajax functionality to our application, however, we can avoid redisplaying the entire page when a user clicks a contact group.</span></span> <span data-ttu-id="d9aa6-164">我們不需要再擷取從資料庫連絡人的群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-164">We no longer need to grab the contact groups from the database.</span></span> <span data-ttu-id="d9aa6-165">我們也不 t 需要透過網路將整個 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-165">We also don t need to push the entire Index view across the wire.</span></span> <span data-ttu-id="d9aa6-166">利用 Ajax，我們我們的資料庫伺服器必須執行的工作數量，我們減小我們的應用程式所需的網路傳輸量。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-166">By taking advantage of Ajax, we reduce the amount of work that our database server must perform and we reduce the amount of network traffic required by our application.</span></span>

## <a name="don-t-be-afraid-of-ajax"></a><span data-ttu-id="d9aa6-167">不要是怕的 Ajax</span><span class="sxs-lookup"><span data-stu-id="d9aa6-167">Don t be Afraid of Ajax</span></span>

<span data-ttu-id="d9aa6-168">有些開發人員避免使用 Ajax，因為它們會擔心舊版瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-168">Some developers avoid using Ajax because they worry about downlevel browsers.</span></span> <span data-ttu-id="d9aa6-169">他們想要確定其 web 應用程式將仍會運作時不支援 JavaScript 的瀏覽器來存取。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-169">They want to make sure that their web applications will still work when accessed by a browser that does not support JavaScript.</span></span> <span data-ttu-id="d9aa6-170">由於 Ajax 相依於 JavaScript，有些開發人員會避免使用 Ajax。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-170">Because Ajax depends on JavaScript, some developers avoid using Ajax.</span></span>

<span data-ttu-id="d9aa6-171">不過，如果您是實作 Ajax 的方式請小心您可以建置新版和舊版的瀏覽器所使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-171">However, if you are careful about how you implement Ajax then you can build applications that work with both uplevel and downlevel browsers.</span></span> <span data-ttu-id="d9aa6-172">我們的連絡人管理員應用程式以支援 JavaScript 的瀏覽器和瀏覽器不會使用。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-172">Our Contact Manager application will work with browsers that support JavaScript and browsers that do not.</span></span>

<span data-ttu-id="d9aa6-173">如果您使用支援 JavaScript 的瀏覽器的連絡人管理員應用程式，您會有較佳使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-173">If you use the Contact Manager application with a browser that supports JavaScript then you will have a better user experience.</span></span> <span data-ttu-id="d9aa6-174">例如，當您按一下連絡人群組，將會更新只會顯示 [連絡人] 頁面的區域。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-174">For example, when you click a contact group, only the region of the page that displays contacts will be updated.</span></span>

<span data-ttu-id="d9aa6-175">如果相反地，您的瀏覽器不支援 JavaScript （或已停用 JavaScript），會在使用 Contact Manager 應用程式時，您會有較理想的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-175">If, on the other hand, you use the Contact Manager application with a browser that does not support JavaScript (or that has JavaScript disabled) then you will have a slightly less desirable user experience.</span></span> <span data-ttu-id="d9aa6-176">例如，當您按一下連絡人群組，整個索引檢視必須公佈回瀏覽器以顯示符合的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-176">For example, when you click a contact group, the entire Index view must be posted back to the browser in order to display the matching list of contacts.</span></span>

## <a name="adding-the-required-javascript-files"></a><span data-ttu-id="d9aa6-177">新增必要的 JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="d9aa6-177">Adding the Required JavaScript Files</span></span>

<span data-ttu-id="d9aa6-178">我們必須將 Ajax 功能新增至我們的應用程式使用三個 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-178">We'll need to use three JavaScript files to add Ajax functionality to our application.</span></span> <span data-ttu-id="d9aa6-179">這三個檔案會包含在新的 ASP.NET MVC 應用程式的指令碼資料夾中。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-179">All three of these files are included in the Scripts folder of a new ASP.NET MVC application.</span></span>

<span data-ttu-id="d9aa6-180">如果您打算使用 Ajax 應用程式中的多個網頁中再合理納入您的應用程式檢視主版頁面中的所需的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-180">If you plan to use Ajax in multiple pages in your application then it makes sense to include the required JavaScript files in your application s view master page.</span></span> <span data-ttu-id="d9aa6-181">如此一來，JavaScript 檔案將會包含在所有應用程式中的頁面自動。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-181">That way, the JavaScript files will be included in all of the pages in your application automatically.</span></span>

<span data-ttu-id="d9aa6-182">新增下列 JavaScript 包含了內&lt;head&gt;檢視主版頁面的標記：</span><span class="sxs-lookup"><span data-stu-id="d9aa6-182">Add the following JavaScript includes inside the &lt;head&gt; tag of your view master page:</span></span>

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a><span data-ttu-id="d9aa6-183">重構來使用 Ajax 的索引檢視</span><span class="sxs-lookup"><span data-stu-id="d9aa6-183">Refactoring the Index View to use Ajax</span></span>

<span data-ttu-id="d9aa6-184">可讓 s 開始先修改索引檢視，以便按一下連絡人群組只會更新此檢視會顯示連絡人的區域。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-184">Let s start by modifying our Index view so that clicking a contact group only updates the region of the view that displays contacts.</span></span> <span data-ttu-id="d9aa6-185">圖 1 中的紅色方塊包含我們想要更新的區域。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-185">The red box in Figure 1 contains the region that we want to update.</span></span>


<span data-ttu-id="d9aa6-186">[![更新連絡人](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d9aa6-186">[![Updating only contacts](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)</span></span>

<span data-ttu-id="d9aa6-187">**圖 01**： 更新的連絡人 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d9aa6-187">**Figure 01**: Updating only contacts([Click to view full-size image](iteration-7-add-ajax-functionality-vb/_static/image2.png))</span></span>


<span data-ttu-id="d9aa6-188">第一個步驟是檢視的個別，我們想要以非同步方式更新至不同的部分 （檢視使用者控制項） 的一部分。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-188">The first step is to separate the part of the view that we want to update asynchronously into a separate partial (view user control).</span></span> <span data-ttu-id="d9aa6-189">顯示連絡人的 索引 檢視的區段已移至 清單 1 部分。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-189">The section of the Index view that displays the table of contacts has been moved into the partial in Listing 1.</span></span>

<span data-ttu-id="d9aa6-190">**列表 1-Views\Contact\ContactList.ascx**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-190">**Listing 1 - Views\Contact\ContactList.ascx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

<span data-ttu-id="d9aa6-191">請注意，在 列表 1 中的部分有索引檢視與不同的模型。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-191">Notice that the partial in Listing 1 has a different model than the Index view.</span></span> <span data-ttu-id="d9aa6-192">*Inherits*屬性中&lt;%@ %頁&gt;指示詞會指定部分繼承 ViewUserControl&lt;群組&gt;類別。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-192">The *Inherits* attribute in the &lt;%@ Page %&gt; directive specifies that the partial inherits from the ViewUserControl&lt;Group&gt; class.</span></span>

<span data-ttu-id="d9aa6-193">已更新的 索引 檢視會包含在 列表 2。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-193">The updated Index view is contained in Listing 2.</span></span>

<span data-ttu-id="d9aa6-194">**列表 2-Views\Contact\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-194">**Listing 2 - Views\Contact\Index.aspx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

<span data-ttu-id="d9aa6-195">有兩件事，您應該注意到關於列表 2 中更新的檢視。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-195">There are two things that you should notice about the updated view in Listing 2.</span></span> <span data-ttu-id="d9aa6-196">首先，請注意，所有的內容移入部分取代 Html.RenderPartial() 呼叫。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-196">First, notice that all of the content moved into the partial is replaced with a call to Html.RenderPartial().</span></span> <span data-ttu-id="d9aa6-197">第一次要求 [索引] 檢視以顯示一組初始的連絡人時，會呼叫 Html.RenderPartial() 方法。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-197">The Html.RenderPartial() method is called when the Index view is first requested in order to display the initial set of contacts.</span></span>

<span data-ttu-id="d9aa6-198">其次，要請您注意的是用來顯示連絡人群組 Html.ActionLink() 已被取代 Ajax.ActionLink()。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-198">Second, notice that the Html.ActionLink() used to display contact groups has been replaced with an Ajax.ActionLink().</span></span> <span data-ttu-id="d9aa6-199">Ajax.ActionLink() 呼叫具備下列參數：</span><span class="sxs-lookup"><span data-stu-id="d9aa6-199">The Ajax.ActionLink() is called with the following parameters:</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

<span data-ttu-id="d9aa6-200">第一個參數表示要顯示連結的文字，第二個參數代表路由值，而第三個參數代表的 Ajax 選項。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-200">The first parameter represents the text to display for the link, the second parameter represents the route values, and the third parameter represents the Ajax options.</span></span> <span data-ttu-id="d9aa6-201">在此情況下，我們使用 UpdateTargetId Ajax 選項指向 HTML &lt;div&gt;我們想要在 Ajax 要求完成之後更新的標記。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-201">In this case, we use the UpdateTargetId Ajax option to point to the HTML &lt;div&gt; tag that we want to update after the Ajax request completes.</span></span> <span data-ttu-id="d9aa6-202">我們想要更新&lt;div&gt;標記，並且具有新的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-202">We want to update the &lt;div&gt; tag with the new list of contacts.</span></span>

<span data-ttu-id="d9aa6-203">已更新的 index （） 方法，請連絡控制站都包含在 列表 3。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-203">The updated Index() method of the Contact controller is contained in Listing 3.</span></span>

<span data-ttu-id="d9aa6-204">**列表 3-Controllers\ContactController.vb （Index 方法）**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-204">**Listing 3 - Controllers\ContactController.vb (Index method)**</span></span>

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

<span data-ttu-id="d9aa6-205">更新的 index （） 動作有條件地傳回下列其中一種。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-205">The updated Index() action conditionally returns one of two things.</span></span> <span data-ttu-id="d9aa6-206">如果 index （） 動作會叫用 Ajax 要求的控制器就會傳回部分。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-206">If the Index() action is invoked by an Ajax request then the controller returns a partial.</span></span> <span data-ttu-id="d9aa6-207">否則，index （） 動作會傳回整個檢視。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-207">Otherwise, the Index() action returns an entire view.</span></span>

<span data-ttu-id="d9aa6-208">請注意，index （） 動作不需要傳回的資料量時叫用 Ajax 要求。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-208">Notice that the Index() action does not need to return as much data when invoked by an Ajax request.</span></span> <span data-ttu-id="d9aa6-209">在一般的要求內容中，索引動作會傳回所有連絡人的群組和選取的連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-209">In the context of a normal request, the Index action returns a list of all of the contact groups and the selected contact group.</span></span> <span data-ttu-id="d9aa6-210">在 Ajax 要求的內容中，index （） 動作會傳回選取的群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-210">In the context of an Ajax request, the Index() action returns only the selected group.</span></span> <span data-ttu-id="d9aa6-211">Ajax 表示較少的工作，在您的資料庫伺服器上。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-211">Ajax means less work on your database server.</span></span>

<span data-ttu-id="d9aa6-212">我們已修改的索引檢視的運作方式在新版和舊版的瀏覽器的情況下。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-212">Our modified Index view works in the case of both uplevel and downlevel browsers.</span></span> <span data-ttu-id="d9aa6-213">如果您按一下連絡人群組，而且您的瀏覽器支援 JavaScript，只有包含連絡人清單的檢視區域會更新。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-213">If you click a contact group, and your browser supports JavaScript, then only the region of the view that contains the list of contacts is updated.</span></span> <span data-ttu-id="d9aa6-214">如果相反地，您的瀏覽器不支援 JavaScript，則會更新整個檢視。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-214">If, on the other hand, your browser does not support JavaScript, then the entire view is updated.</span></span>


<span data-ttu-id="d9aa6-215">我們已更新的 [索引] 檢視有一個問題。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-215">Our updated Index view has one problem.</span></span> <span data-ttu-id="d9aa6-216">當您按一下連絡人群組時，是不反白顯示選取的群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-216">When you click a contact group, the selected group is not highlighted.</span></span> <span data-ttu-id="d9aa6-217">因為群組的清單會顯示為 Ajax 要求期間，會更新區域之外，不會不反白顯示正確的群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-217">Because the list of groups is displayed outside of the region that is updated during an Ajax request, the right group does not get highlighted.</span></span> <span data-ttu-id="d9aa6-218">我們將修正此問題下, 一節。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-218">We'll fix this issue in the next section.</span></span>


## <a name="adding-jquery-animation-effects"></a><span data-ttu-id="d9aa6-219">加入 jQuery 動畫效果</span><span class="sxs-lookup"><span data-stu-id="d9aa6-219">Adding jQuery Animation Effects</span></span>

<span data-ttu-id="d9aa6-220">一般來說，當您按一下連結，以在網頁中的，您可以使用瀏覽器進度列來偵測在瀏覽器正在主動擷取更新的內容。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-220">Normally, when you click a link in a web page, you can use the browser progress bar to detect whether or not the browser is actively fetching the updated content.</span></span> <span data-ttu-id="d9aa6-221">執行時的 Ajax 要求，相反地，在瀏覽器進度列不會顯示任何進度。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-221">When performing an Ajax request, on the other hand, the browser progress bar does not show any progress.</span></span> <span data-ttu-id="d9aa6-222">這可讓使用者感到不安。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-222">This can make users nervous.</span></span> <span data-ttu-id="d9aa6-223">如何知道是否有凍結的瀏覽器？</span><span class="sxs-lookup"><span data-stu-id="d9aa6-223">How do you know whether the browser has frozen?</span></span>

<span data-ttu-id="d9aa6-224">有數種方式，您可以指出要執行的 Ajax 要求時執行工作。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-224">There are several ways that you can indicate to a user that work is being performed while performing an Ajax request.</span></span> <span data-ttu-id="d9aa6-225">其中一個方法是顯示簡單的動畫。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-225">One approach is to display a simple animation.</span></span> <span data-ttu-id="d9aa6-226">例如，您可以淡出區域時的 Ajax 要求開始，並要求完成時，在區域淡出。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-226">For example, you can fade out a region when an Ajax request begins and fade in the region when the request completes.</span></span>

<span data-ttu-id="d9aa6-227">我們將使用隨附於 Microsoft ASP.NET MVC 架構，來建立動畫效果的 jQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-227">We'll use the jQuery library which is included with the Microsoft ASP.NET MVC framework, to create the animation effects.</span></span> <span data-ttu-id="d9aa6-228">已更新的 索引 檢視包含在 列表 4 中。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-228">The updated Index view is contained in Listing 4.</span></span>

<span data-ttu-id="d9aa6-229">**列表 4-Views\Contact\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-229">**Listing 4 - Views\Contact\Index.aspx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

<span data-ttu-id="d9aa6-230">請注意更新的 [索引] 檢視包含三個新的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-230">Notice that the updated Index view contains three new JavaScript functions.</span></span> <span data-ttu-id="d9aa6-231">前兩個函式會使用 jQuery 淡出及淡入的連絡人清單，當您按一下 新連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-231">The first two functions use jQuery to fade out and fade in the list of contacts when you click a new contact group.</span></span> <span data-ttu-id="d9aa6-232">第三個函式會顯示錯誤訊息時的 Ajax 要求結果，造成錯誤 （例如網路逾時）。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-232">The third function displays an error message when an Ajax request results in an error (for example, network timeout).</span></span>

<span data-ttu-id="d9aa6-233">第一個函式也會負責反白顯示選取的群組。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-233">The first function also takes care of highlighting the selected group.</span></span> <span data-ttu-id="d9aa6-234">類別 = 選取的屬性加入至按下之項目的父項目 （LI 項目）。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-234">A class= selected attribute is added to the parent element (the LI element) of the element clicked.</span></span> <span data-ttu-id="d9aa6-235">同樣地，jQuery 輕鬆選取正確的項目並加入的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-235">Again, jQuery makes it easy to select the right element and add the CSS class.</span></span>

<span data-ttu-id="d9aa6-236">這些指令碼會繫結至 Ajax.ActionLink() AjaxOptions 參數的協助的群組連結。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-236">These scripts are tied to the group links with the help of the Ajax.ActionLink() AjaxOptions parameter.</span></span> <span data-ttu-id="d9aa6-237">已更新的 Ajax.ActionLink() 方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d9aa6-237">The updated Ajax.ActionLink() method call looks like this:</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a><span data-ttu-id="d9aa6-238">加入瀏覽器歷程記錄支援</span><span class="sxs-lookup"><span data-stu-id="d9aa6-238">Adding Browser History Support</span></span>

<span data-ttu-id="d9aa6-239">一般來說，當您按一下 [更新] 頁面的連結時，會更新您的瀏覽器歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-239">Normally, when you click a link to update a page, your browser history is updated.</span></span> <span data-ttu-id="d9aa6-240">如此一來，您可以按一下 [瀏覽器上一頁] 按鈕，將先前的狀態之頁面的時間移回。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-240">That way, you can click the browser Back button to move back in time to the previous state of the page.</span></span> <span data-ttu-id="d9aa6-241">例如，如果您按一下朋友連絡人群組，然後按一下 商務連絡人群組，您可以按一下 向後巡覽至頁面的狀態時的朋友連絡群組已選取瀏覽器 上一頁 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-241">For example, if you click the Friends contact group and then click the Business contact group, you can click the browser Back button to navigate back to the state of the page when the Friends contact group was selected.</span></span>

<span data-ttu-id="d9aa6-242">不幸的是，執行 Ajax 要求不會更新瀏覽器記錄自動。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-242">Unfortunately, performing an Ajax request does not update browser history automatically.</span></span> <span data-ttu-id="d9aa6-243">如果您按一下連絡人群組，和 Ajax 要求擷取相符的連絡人的清單，則不會更新瀏覽器歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-243">If you click a contact group, and the list of matching contacts is retrieved with an Ajax request, then the browser history is not updated.</span></span> <span data-ttu-id="d9aa6-244">若要瀏覽回到連絡人群組，選取新的連絡人群組之後，您無法使用瀏覽器的 [上一頁] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-244">You cannot use the browser Back button to navigate back to a contact group after selecting a new contact group.</span></span>

<span data-ttu-id="d9aa6-245">如果您要讓使用者能夠使用瀏覽器上一頁按鈕執行 Ajax 要求之後您就需要執行多一點的工作。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-245">If you want users to be able to use the browser Back button after performing Ajax requests then you need to perform a little more work.</span></span> <span data-ttu-id="d9aa6-246">您需要利用內建的 ASP.NET AJAX 架構的瀏覽器歷程記錄管理功能。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-246">You need to take advantage of the browser history management functionality built in the ASP.NET AJAX Framework.</span></span>

<span data-ttu-id="d9aa6-247">ASP.NET AJAX 瀏覽器歷程記錄，您需要做三件事：</span><span class="sxs-lookup"><span data-stu-id="d9aa6-247">ASP.NET AJAX browser history, you need to do three things:</span></span>

1. <span data-ttu-id="d9aa6-248">EnableBrowserHistory 屬性設定為 true，以啟用瀏覽器歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-248">Enable Browser History by setting the enableBrowserHistory property to true.</span></span>
2. <span data-ttu-id="d9aa6-249">藉由呼叫 addHistoryPoint() 方法的檢視狀態變更時，請將儲存記錄點。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-249">Save history points when the state of a view changes by calling the addHistoryPoint() method.</span></span>
3. <span data-ttu-id="d9aa6-250">Navigate 事件引發時，請重建檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-250">Reconstruct the state of the view when the navigate event is raised.</span></span>

<span data-ttu-id="d9aa6-251">已更新的 索引 檢視會包含在 列表 5。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-251">The updated Index view is contained in Listing 5.</span></span>

<span data-ttu-id="d9aa6-252">**列表 5-Views\Contact\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-252">**Listing 5 - Views\Contact\Index.aspx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

<span data-ttu-id="d9aa6-253">表 5 中，已啟用 pageInit() 函式中的瀏覽器歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-253">In Listing 5, Browser History is enabled in the pageInit() function.</span></span> <span data-ttu-id="d9aa6-254">PageInit() 函式也會設為 navigate 事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-254">The pageInit() function is also used to set up the event handler for the navigate event.</span></span> <span data-ttu-id="d9aa6-255">當瀏覽器下一頁或上一頁 按鈕會造成的頁面來變更狀態時，會引發 navigate 事件。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-255">The navigate event is raised whenever the browser Forward or Back button causes the state of the page to change.</span></span>

<span data-ttu-id="d9aa6-256">當您按一下 連絡人群組，稱為 beginContactList() 方法。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-256">The beginContactList() method is called when you click a contact group.</span></span> <span data-ttu-id="d9aa6-257">這個方法會藉由呼叫 addHistoryPoint() 方法建立新的記錄點。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-257">This method creates a new history point by calling the addHistoryPoint() method.</span></span> <span data-ttu-id="d9aa6-258">按一下連絡人群組的識別碼會新增至歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-258">The id of the contact group clicked is added to history.</span></span>

<span data-ttu-id="d9aa6-259">群組識別碼被擷取自 expando 屬性 [連絡人群組] 連結。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-259">The group id is retrieved from an expando attribute on the contact group link.</span></span> <span data-ttu-id="d9aa6-260">使用下列呼叫來 Ajax.ActionLink() 呈現連結。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-260">The link is rendered with the following call to Ajax.ActionLink().</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

<span data-ttu-id="d9aa6-261">最後一個參數傳遞至 Ajax.ActionLink() 新增名為 groupid （小寫的 XHTML 相容性） 連結的 expando 屬性。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-261">The last parameter passed to the Ajax.ActionLink() adds an expando attribute named groupid to the link (lowercase for XHTML compatibility).</span></span>

<span data-ttu-id="d9aa6-262">當使用者叫用的瀏覽器上一頁或下一頁按鈕時，瀏覽事件引發時，並呼叫 navigate() 方法。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-262">When a user hits the browser Back or Forward button, the navigate event is raised and the navigate() method is called.</span></span> <span data-ttu-id="d9aa6-263">這個方法會更新以符合對應到傳遞至 navigate 方法瀏覽器歷程記錄點之頁面的狀態 頁面中顯示的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-263">This method updates the contacts displayed in the page to match the state of the page that corresponds to the browser history point passed to the navigate method.</span></span>

## <a name="performing-ajax-deletes"></a><span data-ttu-id="d9aa6-264">執行 Ajax 刪除</span><span class="sxs-lookup"><span data-stu-id="d9aa6-264">Performing Ajax Deletes</span></span>

<span data-ttu-id="d9aa6-265">目前，若要刪除的連絡人，您必須按一下 刪除 連結，然後按一下 刪除 確認頁面中顯示的 刪除 按鈕 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-265">Currently, in order to delete a contact, you need to click on the Delete link and then click the Delete button displayed in the delete confirmation page (see Figure 2).</span></span> <span data-ttu-id="d9aa6-266">這似乎是相當繁重的頁面要求執行一些簡單，例如刪除資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-266">This seems like a lot of page requests to do something simple like deleting a database record.</span></span>


<span data-ttu-id="d9aa6-267">[![刪除確認頁面](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d9aa6-267">[![The delete confirmation page](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)</span></span>

<span data-ttu-id="d9aa6-268">**圖 02**： 刪除確認頁面 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="d9aa6-268">**Figure 02**: The delete confirmation page([Click to view full-size image](iteration-7-add-ajax-functionality-vb/_static/image4.png))</span></span>


<span data-ttu-id="d9aa6-269">相當吸引人，請略過刪除確認頁面，並直接從 [索引] 檢視中刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-269">It is tempting to skip the delete confirmation page and delete a contact directly from the Index view.</span></span> <span data-ttu-id="d9aa6-270">您應該避免起舞，因為這種方式開啟您的應用程式的安全性漏洞。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-270">You should avoid this temptation because taking this approach opens your application to security holes.</span></span> <span data-ttu-id="d9aa6-271">一般情況下，您不要想要叫用動作來修改您的 web 應用程式的狀態時，執行 HTTP GET 作業。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-271">In general, you don t want to perform an HTTP GET operation when invoking an action that modifies the state of your web application.</span></span> <span data-ttu-id="d9aa6-272">在執行刪除時，您會想要執行 HTTP POST，或最好使用 HTTP DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-272">When performing a delete, you want to perform an HTTP POST, or better yet, an HTTP DELETE operation.</span></span>

<span data-ttu-id="d9aa6-273">[刪除] 連結會包含在部分 ContactList。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-273">The Delete link is contained in the ContactList partial.</span></span> <span data-ttu-id="d9aa6-274">部分 ContactList 的更新的版本都包含在 列表 6。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-274">An updated version of the ContactList partial is contained in Listing 6.</span></span>

<span data-ttu-id="d9aa6-275">**列表 6-Views\Contact\ContactList.ascx**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-275">**Listing 6 - Views\Contact\ContactList.ascx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

<span data-ttu-id="d9aa6-276">刪除連結會轉譯下列 Ajax.ImageActionLink() 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="d9aa6-276">The Delete link is rendered with the following call to the Ajax.ImageActionLink() method:</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> <span data-ttu-id="d9aa6-277">Ajax.ImageActionLink() 不是 ASP.NET MVC framework 的標準部分。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-277">The Ajax.ImageActionLink() is not a standard part of the ASP.NET MVC framework.</span></span> <span data-ttu-id="d9aa6-278">Ajax.ImageActionLink() 是 Contactmanager 專案中包含自訂的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-278">The Ajax.ImageActionLink() is a custom helper methods included in the Contact Manager project.</span></span>


<span data-ttu-id="d9aa6-279">AjaxOptions 參數有兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-279">The AjaxOptions parameter has two properties.</span></span> <span data-ttu-id="d9aa6-280">首先，確認屬性用來顯示快顯 JavaScript 確認對話方塊。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-280">First, the Confirm property is used to display a popup JavaScript confirmation dialog.</span></span> <span data-ttu-id="d9aa6-281">第二，HttpMethod 屬性用來執行 HTTP DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-281">Second, the HttpMethod property is used to perform an HTTP DELETE operation.</span></span>

<span data-ttu-id="d9aa6-282">列表 7 包含新的 AjaxDelete() 動作已新增至連絡人控制器。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-282">Listing 7 contains a new AjaxDelete() action that has been added to the Contact controller.</span></span>

<span data-ttu-id="d9aa6-283">**列表 7-Controllers\ContactController.vb (AjaxDelete)**</span><span class="sxs-lookup"><span data-stu-id="d9aa6-283">**Listing 7 - Controllers\ContactController.vb (AjaxDelete)**</span></span>   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

<span data-ttu-id="d9aa6-284">AcceptVerbs 屬性裝飾 AjaxDelete() 動作。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-284">The AjaxDelete() action is decorated with an AcceptVerbs attribute.</span></span> <span data-ttu-id="d9aa6-285">這個屬性會防止以外的 HTTP DELETE 作業以外的任何 HTTP 作業所叫用動作。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-285">This attribute prevents the action from being invoked except by any HTTP operation other than an HTTP DELETE operation.</span></span> <span data-ttu-id="d9aa6-286">特別是，您無法叫用此動作，使用 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-286">In particular, you cannot invoke this action with an HTTP GET.</span></span>

<span data-ttu-id="d9aa6-287">刪除資料庫記錄之後，您需要顯示更新的連絡人清單不包含已刪除的資料錄。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-287">After you delete database record, you need to display the updated list of contacts that does not contain the deleted record.</span></span> <span data-ttu-id="d9aa6-288">AjaxDelete() 方法會傳回部分 ContactList 和更新的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-288">The AjaxDelete() method returns the ContactList partial and the updated list of contacts.</span></span>

## <a name="summary"></a><span data-ttu-id="d9aa6-289">總結</span><span class="sxs-lookup"><span data-stu-id="d9aa6-289">Summary</span></span>

<span data-ttu-id="d9aa6-290">這個反覆項目，在中，我們會新增 Ajax 功能到我們的連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-290">In this iteration, we added Ajax functionality to our Contact Manager application.</span></span> <span data-ttu-id="d9aa6-291">我們使用 Ajax，以改善回應性和我們的應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-291">We used Ajax to improve the responsiveness and performance of our application.</span></span>

<span data-ttu-id="d9aa6-292">首先，我們重構 [索引] 檢視，以便按一下連絡人群組，不會更新整個檢視。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-292">First, we refactored the Index view so that clicking a contact group does not update the entire view.</span></span> <span data-ttu-id="d9aa6-293">相反地，按一下連絡人群組只會更新連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-293">Instead, clicking a contact group only updates the list of contacts.</span></span>

<span data-ttu-id="d9aa6-294">接下來，我們使用 jQuery 動畫效果淡出及淡入的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-294">Next, we used jQuery animation effects to fade out and fade in the list of contacts.</span></span> <span data-ttu-id="d9aa6-295">將動畫新增至 Ajax 應用程式可以用來提供應用程式的瀏覽器進度列的對等的使用者。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-295">Adding animation to an Ajax application can be used to provide users of the application with the equivalent of a browser progress bar.</span></span>

<span data-ttu-id="d9aa6-296">我們也會加入至我們的 Ajax 應用程式的瀏覽器歷程記錄 」 支援。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-296">We also added browser history support to our Ajax application.</span></span> <span data-ttu-id="d9aa6-297">我們已啟用使用者按一下 [瀏覽器上一頁及下一頁按鈕來變更索引] 檢視的狀態。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-297">We enabled users to click the browser Back and Forward buttons to change the state of the Index view.</span></span>

<span data-ttu-id="d9aa6-298">最後，我們會建立支援 HTTP DELETE 作業的刪除連結。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-298">Finally, we created a delete link that supports HTTP DELETE operations.</span></span> <span data-ttu-id="d9aa6-299">藉由執行 Ajax 刪除，我們可以讓使用者能夠刪除資料庫中的記錄，而不需要使用者要求額外的刪除確認頁面。</span><span class="sxs-lookup"><span data-stu-id="d9aa6-299">By performing Ajax deletes, we enable users to delete database records without requiring the user to request an additional delete confirmation page.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d9aa6-300">上一步</span><span class="sxs-lookup"><span data-stu-id="d9aa6-300">Previous</span></span>](iteration-6-use-test-driven-development-vb.md)
