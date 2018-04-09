---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: '反覆項目 #7 – 新增 Ajax 功能 (C#) |Microsoft 文件'
author: microsoft
description: 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 35d62383a571725749b2fc629bbb17954657b2f6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="iteration-7--add-ajax-functionality-c"></a><span data-ttu-id="10f4a-103">反覆項目 #7 – 新增 Ajax 功能 (C#)</span><span class="sxs-lookup"><span data-stu-id="10f4a-103">Iteration #7 – Add Ajax functionality (C#)</span></span>
====================
<span data-ttu-id="10f4a-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="10f4a-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="10f4a-105">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="10f4a-105">Download Code</span></span>](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> <span data-ttu-id="10f4a-106">在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="10f4a-106">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="10f4a-107">建立連絡人管理 ASP.NET MVC 應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="10f4a-107">Building a Contact Management ASP.NET MVC Application (C#)</span></span>

<span data-ttu-id="10f4a-108">在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="10f4a-108">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="10f4a-109">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-109">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="10f4a-110">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="10f4a-110">We build the application over multiple iterations.</span></span> <span data-ttu-id="10f4a-111">與每個反覆項目，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-111">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="10f4a-112">這個多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="10f4a-112">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="10f4a-113">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-113">Iteration #1 - Create the application.</span></span> <span data-ttu-id="10f4a-114">在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-114">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="10f4a-115">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="10f4a-115">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="10f4a-116">反覆項目 #2-請看起來很棒的應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-116">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="10f4a-117">在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="10f4a-117">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="10f4a-118">反覆項目 #3-加入表單驗證。</span><span class="sxs-lookup"><span data-stu-id="10f4a-118">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="10f4a-119">第三個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="10f4a-119">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="10f4a-120">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="10f4a-120">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="10f4a-121">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="10f4a-121">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="10f4a-122">反覆項目 #4-請鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-122">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="10f4a-123">在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-123">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="10f4a-124">例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-124">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="10f4a-125">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="10f4a-125">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="10f4a-126">第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="10f4a-126">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="10f4a-127">我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="10f4a-127">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="10f4a-128">反覆項目 #6-使用測試為導向的開發。</span><span class="sxs-lookup"><span data-stu-id="10f4a-128">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="10f4a-129">這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="10f4a-129">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="10f4a-130">在這個反覆項目，我們會加入連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-130">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="10f4a-131">反覆項目 #7： 加入 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-131">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="10f4a-132">在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="10f4a-132">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="10f4a-133">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="10f4a-133">This Iteration</span></span>

<span data-ttu-id="10f4a-134">在連絡人管理員應用程式的這個反覆項目，我們可以重構我們的應用程式來使用 Ajax。</span><span class="sxs-lookup"><span data-stu-id="10f4a-134">In this iteration of the Contact Manager application, we refactor our application to make use of Ajax.</span></span> <span data-ttu-id="10f4a-135">利用 Ajax，就能讓我們的應用程式更能有效回應。</span><span class="sxs-lookup"><span data-stu-id="10f4a-135">By taking advantage of Ajax, we make our application more responsive.</span></span> <span data-ttu-id="10f4a-136">我們可以避免當我們需要更新只有特定區域在頁面中的時呈現整個頁面。</span><span class="sxs-lookup"><span data-stu-id="10f4a-136">We can avoid rendering an entire page when we need to update only a certain region in a page.</span></span>

<span data-ttu-id="10f4a-137">我們將會重構我們索引檢視，以便我們不 t 需要每當有人選取新的連絡人群組時，重新顯示整個頁面。</span><span class="sxs-lookup"><span data-stu-id="10f4a-137">We'll refactor our Index view so that we don t need to redisplay the entire page whenever someone selects a new contact group.</span></span> <span data-ttu-id="10f4a-138">相反地，當使用者按一下連絡人群組，我們將只更新的連絡人清單，然後將其餘的頁面單獨。</span><span class="sxs-lookup"><span data-stu-id="10f4a-138">Instead, when someone clicks a contact group, we'll just update the list of contacts and leave the rest of the page alone.</span></span>

<span data-ttu-id="10f4a-139">我們也將會變更我們刪除連結運作的方式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-139">We'll also change the way our delete link works.</span></span> <span data-ttu-id="10f4a-140">而不是顯示個別的確認頁面，我們將會顯示 JavaScript 確認對話方塊。</span><span class="sxs-lookup"><span data-stu-id="10f4a-140">Instead of displaying a separate confirmation page, we'll display a JavaScript confirmation dialog.</span></span> <span data-ttu-id="10f4a-141">如果您確認您想要刪除的連絡人，針對要從資料庫刪除的連絡人記錄之伺服器執行 HTTP DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="10f4a-141">If you confirm that you want to delete a contact, an HTTP DELETE operation is performed against the server to delete the contact record from the database.</span></span>

<span data-ttu-id="10f4a-142">此外，我們也會利用 jQuery 動畫效果加入我們的索引檢視。</span><span class="sxs-lookup"><span data-stu-id="10f4a-142">Furthermore, we will take advantage of jQuery to add animation effects to our Index view.</span></span> <span data-ttu-id="10f4a-143">正在從伺服器擷取新的連絡人清單時，我們將會顯示動畫。</span><span class="sxs-lookup"><span data-stu-id="10f4a-143">We'll display an animation when the new list of contacts is being fetched from the server.</span></span>

<span data-ttu-id="10f4a-144">最後，我們也會利用管理瀏覽器歷程記錄的 ASP.NET AJAX framework 支援。</span><span class="sxs-lookup"><span data-stu-id="10f4a-144">Finally, we'll take advantage of the ASP.NET AJAX framework support for managing browser history.</span></span> <span data-ttu-id="10f4a-145">我們執行更新連絡人清單的 Ajax 呼叫時，我們將建立記錄點。</span><span class="sxs-lookup"><span data-stu-id="10f4a-145">We'll create history points whenever we perform an Ajax call to update the contact list.</span></span> <span data-ttu-id="10f4a-146">這樣一來，瀏覽器向後和向前按鈕將會運作。</span><span class="sxs-lookup"><span data-stu-id="10f4a-146">That way, the browser backward and forward buttons will work.</span></span>

## <a name="why-use-ajax"></a><span data-ttu-id="10f4a-147">為何要使用 Ajax？</span><span class="sxs-lookup"><span data-stu-id="10f4a-147">Why use Ajax?</span></span>

<span data-ttu-id="10f4a-148">使用 Ajax 有許多好處。</span><span class="sxs-lookup"><span data-stu-id="10f4a-148">Using Ajax has many benefits.</span></span> <span data-ttu-id="10f4a-149">首先，將 Ajax 功能加入至應用程式會導致較佳使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="10f4a-149">First, adding Ajax functionality to an application results in a better user experience.</span></span> <span data-ttu-id="10f4a-150">在一般 web 應用程式中，整個頁面必須回傳至伺服器，每次使用者執行的動作。</span><span class="sxs-lookup"><span data-stu-id="10f4a-150">In a normal web application, the entire page must be posted back to the server each and every time a user performs an action.</span></span> <span data-ttu-id="10f4a-151">每當您執行的動作時，瀏覽器鎖定且使用者必須等到整個頁面提取，且會重新顯示。</span><span class="sxs-lookup"><span data-stu-id="10f4a-151">Whenever you perform an action, the browser locks and the user must wait until the entire page is fetched and redisplayed.</span></span>

<span data-ttu-id="10f4a-152">這會在桌面應用程式的情況下，無法接受的體驗。</span><span class="sxs-lookup"><span data-stu-id="10f4a-152">This would be an unacceptable experience in the case of a desktop application.</span></span> <span data-ttu-id="10f4a-153">不過，傳統上，我們都和 web 應用程式在此不正確的使用者經驗因為我們不知道我們可以執行更好。</span><span class="sxs-lookup"><span data-stu-id="10f4a-153">But, traditionally, we lived with this bad user experience in the case of a web application because we did not know that we could do any better.</span></span> <span data-ttu-id="10f4a-154">我們想的那麼 web 應用程式的限制時，實際上，它只我們 imaginations 的限制。</span><span class="sxs-lookup"><span data-stu-id="10f4a-154">We thought it was a limitation of web applications when, in actuality, it was just a limitation of our imaginations.</span></span>

<span data-ttu-id="10f4a-155">Ajax 應用程式中，您不會導致使用者體驗只是為了更新頁面中止 t 需要。</span><span class="sxs-lookup"><span data-stu-id="10f4a-155">In an Ajax application, you don t need to bring the user experience to a halt just to update a page.</span></span> <span data-ttu-id="10f4a-156">相反地，您可以更新頁面背景中執行的非同步要求。</span><span class="sxs-lookup"><span data-stu-id="10f4a-156">Instead, you can perform an asynchronous request in the background to update the page.</span></span> <span data-ttu-id="10f4a-157">您不 t 強制使用者稍候網頁的一部分取得更新。</span><span class="sxs-lookup"><span data-stu-id="10f4a-157">You don t force the user to wait while part of the page gets updated.</span></span>

<span data-ttu-id="10f4a-158">利用 Ajax，您也可以改善應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-158">By taking advantage of Ajax, you also can improve the performance of your application.</span></span> <span data-ttu-id="10f4a-159">請考慮連絡人管理員應用程式的運作方式現在不使用 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-159">Consider how the Contact Manager application works right now without Ajax functionality.</span></span> <span data-ttu-id="10f4a-160">當您按一下連絡人群組時，就必須重新顯示整個索引檢視。</span><span class="sxs-lookup"><span data-stu-id="10f4a-160">When you click a contact group, the entire Index view must be redisplayed.</span></span> <span data-ttu-id="10f4a-161">必須從資料庫伺服器擷取的連絡人清單以及連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-161">The list of contacts and list of contact groups must be retrieved from the database server.</span></span> <span data-ttu-id="10f4a-162">所有資料必須傳遞透過網路從 web 伺服器到網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="10f4a-162">All of this data must be passed across the wire from web server to web browser.</span></span>

<span data-ttu-id="10f4a-163">我們將 Ajax 功能新增至我們的應用程式之後，不過，我們就可以避免重新顯示整個頁面中，當使用者按一下連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-163">After we add Ajax functionality to our application, however, we can avoid redisplaying the entire page when a user clicks a contact group.</span></span> <span data-ttu-id="10f4a-164">我們不需要再擷取從資料庫連絡人的群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-164">We no longer need to grab the contact groups from the database.</span></span> <span data-ttu-id="10f4a-165">我們也不 t 需要跨線路推送整個索引檢視。</span><span class="sxs-lookup"><span data-stu-id="10f4a-165">We also don t need to push the entire Index view across the wire.</span></span> <span data-ttu-id="10f4a-166">利用 Ajax，我們必須先執行我們的資料庫伺服器的工作量，我們減小我們的應用程式所需的網路傳輸量。</span><span class="sxs-lookup"><span data-stu-id="10f4a-166">By taking advantage of Ajax, we reduce the amount of work that our database server must perform and we reduce the amount of network traffic required by our application.</span></span>

## <a name="don-t-be-afraid-of-ajax"></a><span data-ttu-id="10f4a-167">不要被恐怕 Ajax</span><span class="sxs-lookup"><span data-stu-id="10f4a-167">Don t be Afraid of Ajax</span></span>

<span data-ttu-id="10f4a-168">有些開發人員會避免使用 Ajax，因為它們擔心舊版瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="10f4a-168">Some developers avoid using Ajax because they worry about downlevel browsers.</span></span> <span data-ttu-id="10f4a-169">他們想要確定其 web 應用程式將仍然運作時不支援 JavaScript 的瀏覽器來存取。</span><span class="sxs-lookup"><span data-stu-id="10f4a-169">They want to make sure that their web applications will still work when accessed by a browser that does not support JavaScript.</span></span> <span data-ttu-id="10f4a-170">由於 Ajax 相依於 JavaScript 中，有些開發人員會避免使用 Ajax。</span><span class="sxs-lookup"><span data-stu-id="10f4a-170">Because Ajax depends on JavaScript, some developers avoid using Ajax.</span></span>

<span data-ttu-id="10f4a-171">不過，如果您是您如何實作 Ajax 時請小心您可以建立使用新版和舊版瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-171">However, if you are careful about how you implement Ajax then you can build applications that work with both uplevel and downlevel browsers.</span></span> <span data-ttu-id="10f4a-172">我們的連絡人管理員應用程式將會使用支援 JavaScript 的瀏覽器和瀏覽器的不相符。</span><span class="sxs-lookup"><span data-stu-id="10f4a-172">Our Contact Manager application will work with browsers that support JavaScript and browsers that do not.</span></span>

<span data-ttu-id="10f4a-173">如果您使用連絡人管理員應用程式的瀏覽器支援 JavaScript，則您將有較佳使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="10f4a-173">If you use the Contact Manager application with a browser that supports JavaScript then you will have a better user experience.</span></span> <span data-ttu-id="10f4a-174">例如，當您按一下連絡人群組，只顯示連絡人的網頁的區域將會更新。</span><span class="sxs-lookup"><span data-stu-id="10f4a-174">For example, when you click a contact group, only the region of the page that displays contacts will be updated.</span></span>

<span data-ttu-id="10f4a-175">如果相反地，您的瀏覽器不支援 JavaScript （或將停用 JavaScript） 使用連絡人管理員應用程式時，您會有稍微較不理想的使用者經驗。</span><span class="sxs-lookup"><span data-stu-id="10f4a-175">If, on the other hand, you use the Contact Manager application with a browser that does not support JavaScript (or that has JavaScript disabled) then you will have a slightly less desirable user experience.</span></span> <span data-ttu-id="10f4a-176">例如，當您按一下連絡人群組，整個索引檢視必須回傳至瀏覽器以顯示符合的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-176">For example, when you click a contact group, the entire Index view must be posted back to the browser in order to display the matching list of contacts.</span></span>

## <a name="adding-the-required-javascript-files"></a><span data-ttu-id="10f4a-177">新增必要的 JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="10f4a-177">Adding the Required JavaScript Files</span></span>

<span data-ttu-id="10f4a-178">我們需要使用三個的 JavaScript 檔案加入至我們的應用程式的 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-178">We'll need to use three JavaScript files to add Ajax functionality to our application.</span></span> <span data-ttu-id="10f4a-179">這三個，這些檔案會包含在新的 ASP.NET MVC 應用程式的指令碼 資料夾。</span><span class="sxs-lookup"><span data-stu-id="10f4a-179">All three of these files are included in the Scripts folder of a new ASP.NET MVC application.</span></span>

<span data-ttu-id="10f4a-180">如果您打算在應用程式中的多個網頁中使用 Ajax 然後合理您應用程式的檢視主版頁面中包含所需的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="10f4a-180">If you plan to use Ajax in multiple pages in your application then it makes sense to include the required JavaScript files in your application s view master page.</span></span> <span data-ttu-id="10f4a-181">這樣一來，JavaScript 檔案將會自動包含在所有應用程式中的頁面。</span><span class="sxs-lookup"><span data-stu-id="10f4a-181">That way, the JavaScript files will be included in all of the pages in your application automatically.</span></span>

<span data-ttu-id="10f4a-182">加入下列 JavaScript 包含內&lt;head&gt;檢視主版頁面的標記：</span><span class="sxs-lookup"><span data-stu-id="10f4a-182">Add the following JavaScript includes inside the &lt;head&gt; tag of your view master page:</span></span>

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a><span data-ttu-id="10f4a-183">重構以使用 Ajax 索引檢視</span><span class="sxs-lookup"><span data-stu-id="10f4a-183">Refactoring the Index View to use Ajax</span></span>

<span data-ttu-id="10f4a-184">可讓 s 一開始先修改我們索引檢視，以便按一下連絡人群組只會更新此檢視會顯示連絡人的區域。</span><span class="sxs-lookup"><span data-stu-id="10f4a-184">Let s start by modifying our Index view so that clicking a contact group only updates the region of the view that displays contacts.</span></span> <span data-ttu-id="10f4a-185">圖 1 中的紅色方塊包含我們想要更新的區域。</span><span class="sxs-lookup"><span data-stu-id="10f4a-185">The red box in Figure 1 contains the region that we want to update.</span></span>


<span data-ttu-id="10f4a-186">[![更新連絡人](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="10f4a-186">[![Updating only contacts](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)</span></span>

<span data-ttu-id="10f4a-187">**圖 01**： 更新的連絡人 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="10f4a-187">**Figure 01**: Updating only contacts([Click to view full-size image](iteration-7-add-ajax-functionality-cs/_static/image2.png))</span></span>


<span data-ttu-id="10f4a-188">第一個步驟是檢視的分隔的部分我們想要以非同步方式更新到不同的部分 （檢視使用者控制項）。</span><span class="sxs-lookup"><span data-stu-id="10f4a-188">The first step is to separate the part of the view that we want to update asynchronously into a separate partial (view user control).</span></span> <span data-ttu-id="10f4a-189">顯示連絡人的索引檢視的區段已移到清單 1 中部分。</span><span class="sxs-lookup"><span data-stu-id="10f4a-189">The section of the Index view that displays the table of contacts has been moved into the partial in Listing 1.</span></span>

<span data-ttu-id="10f4a-190">**Listing 1 - Views\Contact\ContactList.ascx**</span><span class="sxs-lookup"><span data-stu-id="10f4a-190">**Listing 1 - Views\Contact\ContactList.ascx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

<span data-ttu-id="10f4a-191">請注意部分列表 1 中的有不同於索引檢視表的模型。</span><span class="sxs-lookup"><span data-stu-id="10f4a-191">Notice that the partial in Listing 1 has a different model than the Index view.</span></span> <span data-ttu-id="10f4a-192">*Inherits*屬性&lt;%@ 頁面 %&gt;指示詞會指定部分繼承 ViewUserControl&lt;群組&gt;類別。</span><span class="sxs-lookup"><span data-stu-id="10f4a-192">The *Inherits* attribute in the &lt;%@ Page %&gt; directive specifies that the partial inherits from the ViewUserControl&lt;Group&gt; class.</span></span>

<span data-ttu-id="10f4a-193">更新的索引檢視表包含在清單 2。</span><span class="sxs-lookup"><span data-stu-id="10f4a-193">The updated Index view is contained in Listing 2.</span></span>

<span data-ttu-id="10f4a-194">**Listing 2 - Views\Contact\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="10f4a-194">**Listing 2 - Views\Contact\Index.aspx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

<span data-ttu-id="10f4a-195">有兩個的項目，您應該注意到關於更新的檢視表中列出的 2。</span><span class="sxs-lookup"><span data-stu-id="10f4a-195">There are two things that you should notice about the updated view in Listing 2.</span></span> <span data-ttu-id="10f4a-196">首先，請注意，所有內容移動到部分會取代 Html.RenderPartial() 呼叫。</span><span class="sxs-lookup"><span data-stu-id="10f4a-196">First, notice that all of the content moved into the partial is replaced with a call to Html.RenderPartial().</span></span> <span data-ttu-id="10f4a-197">索引檢視第一次要求以顯示一組初始的連絡人時，會呼叫 Html.RenderPartial() 方法。</span><span class="sxs-lookup"><span data-stu-id="10f4a-197">The Html.RenderPartial() method is called when the Index view is first requested in order to display the initial set of contacts.</span></span>

<span data-ttu-id="10f4a-198">第二，請注意，用來顯示連絡人群組 Html.ActionLink() 已被取代 Ajax.ActionLink()。</span><span class="sxs-lookup"><span data-stu-id="10f4a-198">Second, notice that the Html.ActionLink() used to display contact groups has been replaced with an Ajax.ActionLink().</span></span> <span data-ttu-id="10f4a-199">Ajax.ActionLink() 呼叫具備下列參數：</span><span class="sxs-lookup"><span data-stu-id="10f4a-199">The Ajax.ActionLink() is called with the following parameters:</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

<span data-ttu-id="10f4a-200">第一個參數代表要顯示的連結文字、 第二個參數代表的路由值，而第三個參數代表 Ajax 選項。</span><span class="sxs-lookup"><span data-stu-id="10f4a-200">The first parameter represents the text to display for the link, the second parameter represents the route values, and the third parameter represents the Ajax options.</span></span> <span data-ttu-id="10f4a-201">在此情況下，我們使用 UpdateTargetId Ajax 選項以指向 HTML &lt;div&gt;我們想要更新 Ajax 要求完成之後的標記。</span><span class="sxs-lookup"><span data-stu-id="10f4a-201">In this case, we use the UpdateTargetId Ajax option to point to the HTML &lt;div&gt; tag that we want to update after the Ajax request completes.</span></span> <span data-ttu-id="10f4a-202">我們想要更新&lt;div&gt;標記與新的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-202">We want to update the &lt;div&gt; tag with the new list of contacts.</span></span>

<span data-ttu-id="10f4a-203">已更新 index （） 方法的連絡人控制器被包含在列出的 3。</span><span class="sxs-lookup"><span data-stu-id="10f4a-203">The updated Index() method of the Contact controller is contained in Listing 3.</span></span>

<span data-ttu-id="10f4a-204">**列出 3-Controllers\ContactController.cs （索引方法）**</span><span class="sxs-lookup"><span data-stu-id="10f4a-204">**Listing 3 - Controllers\ContactController.cs (Index method)**</span></span>

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

<span data-ttu-id="10f4a-205">更新的 index 動作有條件地傳回其中一個兩件事。</span><span class="sxs-lookup"><span data-stu-id="10f4a-205">The updated Index() action conditionally returns one of two things.</span></span> <span data-ttu-id="10f4a-206">如果 index 動作 Ajax 要求叫用控制器就會傳回部分。</span><span class="sxs-lookup"><span data-stu-id="10f4a-206">If the Index() action is invoked by an Ajax request then the controller returns a partial.</span></span> <span data-ttu-id="10f4a-207">否則，index （） 動作會傳回整個檢視。</span><span class="sxs-lookup"><span data-stu-id="10f4a-207">Otherwise, the Index() action returns an entire view.</span></span>

<span data-ttu-id="10f4a-208">請注意 index 動作不需要傳回 Ajax 要求叫用時，最多資料。</span><span class="sxs-lookup"><span data-stu-id="10f4a-208">Notice that the Index() action does not need to return as much data when invoked by an Ajax request.</span></span> <span data-ttu-id="10f4a-209">在一般的要求內容中，索引動作會傳回所有連絡人群組與所選的連絡人群組的清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-209">In the context of a normal request, the Index action returns a list of all of the contact groups and the selected contact group.</span></span> <span data-ttu-id="10f4a-210">在 Ajax 要求的內容中，index （） 動作會傳回選取的群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-210">In the context of an Ajax request, the Index() action returns only the selected group.</span></span> <span data-ttu-id="10f4a-211">Ajax 中表示您的資料庫伺服器上的工作較少。</span><span class="sxs-lookup"><span data-stu-id="10f4a-211">Ajax means less work on your database server.</span></span>

<span data-ttu-id="10f4a-212">我們已修改的索引檢視的運作方式在新版和舊版瀏覽器的情況下。</span><span class="sxs-lookup"><span data-stu-id="10f4a-212">Our modified Index view works in the case of both uplevel and downlevel browsers.</span></span> <span data-ttu-id="10f4a-213">如果您按一下連絡人群組，而且您的瀏覽器支援 JavaScript，則會更新檢視，其中包含連絡人清單的區域。</span><span class="sxs-lookup"><span data-stu-id="10f4a-213">If you click a contact group, and your browser supports JavaScript, then only the region of the view that contains the list of contacts is updated.</span></span> <span data-ttu-id="10f4a-214">如果相反地，您的瀏覽器不支援 JavaScript，則會更新整個檢視。</span><span class="sxs-lookup"><span data-stu-id="10f4a-214">If, on the other hand, your browser does not support JavaScript, then the entire view is updated.</span></span>


<span data-ttu-id="10f4a-215">更新的索引檢視表有一個問題。</span><span class="sxs-lookup"><span data-stu-id="10f4a-215">Our updated Index view has one problem.</span></span> <span data-ttu-id="10f4a-216">當您按一下連絡人群組時，為非反白顯示所選的群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-216">When you click a contact group, the selected group is not highlighted.</span></span> <span data-ttu-id="10f4a-217">因為更新期間 Ajax 要求區域外顯示群組的清單，正確的群組不會無法取得反白顯示。</span><span class="sxs-lookup"><span data-stu-id="10f4a-217">Because the list of groups is displayed outside of the region that is updated during an Ajax request, the right group does not get highlighted.</span></span> <span data-ttu-id="10f4a-218">我們將在下一節中修正此問題。</span><span class="sxs-lookup"><span data-stu-id="10f4a-218">We'll fix this issue in the next section.</span></span>


## <a name="adding-jquery-animation-effects"></a><span data-ttu-id="10f4a-219">新增 jQuery 動畫效果</span><span class="sxs-lookup"><span data-stu-id="10f4a-219">Adding jQuery Animation Effects</span></span>

<span data-ttu-id="10f4a-220">一般來說，當您按一下連結，以在網頁中，您可以使用瀏覽器進度列來偵測為主動擷取更新的內容瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="10f4a-220">Normally, when you click a link in a web page, you can use the browser progress bar to detect whether or not the browser is actively fetching the updated content.</span></span> <span data-ttu-id="10f4a-221">當執行 Ajax 要求時，相反地，在瀏覽器進度列不會顯示任何進度。</span><span class="sxs-lookup"><span data-stu-id="10f4a-221">When performing an Ajax request, on the other hand, the browser progress bar does not show any progress.</span></span> <span data-ttu-id="10f4a-222">這可讓使用者外人。</span><span class="sxs-lookup"><span data-stu-id="10f4a-222">This can make users nervous.</span></span> <span data-ttu-id="10f4a-223">如何知道是否有凍結的瀏覽器？</span><span class="sxs-lookup"><span data-stu-id="10f4a-223">How do you know whether the browser has frozen?</span></span>

<span data-ttu-id="10f4a-224">有幾種方式，您可以指出使用者執行 Ajax 要求時執行工作。</span><span class="sxs-lookup"><span data-stu-id="10f4a-224">There are several ways that you can indicate to a user that work is being performed while performing an Ajax request.</span></span> <span data-ttu-id="10f4a-225">其中一個方法是顯示簡單的動畫。</span><span class="sxs-lookup"><span data-stu-id="10f4a-225">One approach is to display a simple animation.</span></span> <span data-ttu-id="10f4a-226">例如，您可以淡出區域 Ajax 要求開始與要求完成時，區域淡出。</span><span class="sxs-lookup"><span data-stu-id="10f4a-226">For example, you can fade out a region when an Ajax request begins and fade in the region when the request completes.</span></span>

<span data-ttu-id="10f4a-227">我們會使用隨附於 Microsoft ASP.NET MVC 架構中，若要建立的動畫效果的 jQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="10f4a-227">We'll use the jQuery library which is included with the Microsoft ASP.NET MVC framework, to create the animation effects.</span></span> <span data-ttu-id="10f4a-228">包含更新的索引檢視表中列出的 4。</span><span class="sxs-lookup"><span data-stu-id="10f4a-228">The updated Index view is contained in Listing 4.</span></span>

<span data-ttu-id="10f4a-229">**列出 4-Views\Contact\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="10f4a-229">**Listing 4 - Views\Contact\Index.aspx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

<span data-ttu-id="10f4a-230">請注意更新的索引檢視表包含三個新的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-230">Notice that the updated Index view contains three new JavaScript functions.</span></span> <span data-ttu-id="10f4a-231">前兩個函式會使用 jQuery 淡出及淡入的連絡人清單，當您按一下 新連絡人的群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-231">The first two functions use jQuery to fade out and fade in the list of contacts when you click a new contact group.</span></span> <span data-ttu-id="10f4a-232">第三個函式會顯示錯誤訊息產生 Ajax 要求時，發生錯誤 （例如，網路逾時）。</span><span class="sxs-lookup"><span data-stu-id="10f4a-232">The third function displays an error message when an Ajax request results in an error (for example, network timeout).</span></span>

<span data-ttu-id="10f4a-233">第一個函式也會負責反白顯示選取的群組。</span><span class="sxs-lookup"><span data-stu-id="10f4a-233">The first function also takes care of highlighting the selected group.</span></span> <span data-ttu-id="10f4a-234">類別 = 選取的屬性加入至父項目 （LI 項目） 的已按下的項目。</span><span class="sxs-lookup"><span data-stu-id="10f4a-234">A class= selected attribute is added to the parent element (the LI element) of the element clicked.</span></span> <span data-ttu-id="10f4a-235">同樣地，jQuery 容易，選取正確的項目並加入 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="10f4a-235">Again, jQuery makes it easy to select the right element and add the CSS class.</span></span>

<span data-ttu-id="10f4a-236">這些指令碼會繫結至 Ajax.ActionLink() AjaxOptions 參數搭配群組連結。</span><span class="sxs-lookup"><span data-stu-id="10f4a-236">These scripts are tied to the group links with the help of the Ajax.ActionLink() AjaxOptions parameter.</span></span> <span data-ttu-id="10f4a-237">更新的 Ajax.ActionLink() 方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="10f4a-237">The updated Ajax.ActionLink() method call looks like this:</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a><span data-ttu-id="10f4a-238">加入瀏覽器歷程記錄支援</span><span class="sxs-lookup"><span data-stu-id="10f4a-238">Adding Browser History Support</span></span>

<span data-ttu-id="10f4a-239">一般來說，當您按一下 [更新] 頁面的連結，就會更新您的瀏覽器歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="10f4a-239">Normally, when you click a link to update a page, your browser history is updated.</span></span> <span data-ttu-id="10f4a-240">這樣一來，您可以按一下 [瀏覽器上一頁] 按鈕，以回到時間中先前的頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="10f4a-240">That way, you can click the browser Back button to move back in time to the previous state of the page.</span></span> <span data-ttu-id="10f4a-241">例如，如果您按一下好友連絡人群組，然後按一下 商務連絡人群組，您可以按一下 向後巡覽至之頁面的狀態時未選取的朋友連絡群組瀏覽器 上一頁 按鈕。</span><span class="sxs-lookup"><span data-stu-id="10f4a-241">For example, if you click the Friends contact group and then click the Business contact group, you can click the browser Back button to navigate back to the state of the page when the Friends contact group was selected.</span></span>

<span data-ttu-id="10f4a-242">不幸的是，執行 Ajax 要求不會更新瀏覽器記錄自動。</span><span class="sxs-lookup"><span data-stu-id="10f4a-242">Unfortunately, performing an Ajax request does not update browser history automatically.</span></span> <span data-ttu-id="10f4a-243">如果您按一下連絡人群組，使用 Ajax 要求擷取的比對連絡人清單，然後瀏覽器歷程記錄不會更新。</span><span class="sxs-lookup"><span data-stu-id="10f4a-243">If you click a contact group, and the list of matching contacts is retrieved with an Ajax request, then the browser history is not updated.</span></span> <span data-ttu-id="10f4a-244">若要導覽回到連絡人群組中，選取新的連絡人群組之後，您無法使用瀏覽器的 [上一頁] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="10f4a-244">You cannot use the browser Back button to navigate back to a contact group after selecting a new contact group.</span></span>

<span data-ttu-id="10f4a-245">如果您想要讓使用者能夠使用瀏覽器上一頁按鈕執行 Ajax 要求之後您就需要執行更多的工作。</span><span class="sxs-lookup"><span data-stu-id="10f4a-245">If you want users to be able to use the browser Back button after performing Ajax requests then you need to perform a little more work.</span></span> <span data-ttu-id="10f4a-246">您需要利用內建 ASP.NET AJAX Framework 的瀏覽器歷程記錄管理功能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-246">You need to take advantage of the browser history management functionality built in the ASP.NET AJAX Framework.</span></span>

<span data-ttu-id="10f4a-247">ASP.NET AJAX 瀏覽器歷程記錄，您需要做三件事：</span><span class="sxs-lookup"><span data-stu-id="10f4a-247">ASP.NET AJAX browser history, you need to do three things:</span></span>

1. <span data-ttu-id="10f4a-248">啟用瀏覽器歷程記錄 enableBrowserHistory 屬性設定為 true。</span><span class="sxs-lookup"><span data-stu-id="10f4a-248">Enable Browser History by setting the enableBrowserHistory property to true.</span></span>
2. <span data-ttu-id="10f4a-249">藉由呼叫 addHistoryPoint() 方法的檢視狀態變更時，將儲存記錄點。</span><span class="sxs-lookup"><span data-stu-id="10f4a-249">Save history points when the state of a view changes by calling the addHistoryPoint() method.</span></span>
3. <span data-ttu-id="10f4a-250">巡覽事件引發時，重新建構檢視的狀態。</span><span class="sxs-lookup"><span data-stu-id="10f4a-250">Reconstruct the state of the view when the navigate event is raised.</span></span>

<span data-ttu-id="10f4a-251">包含更新的索引檢視表中列出的 5。</span><span class="sxs-lookup"><span data-stu-id="10f4a-251">The updated Index view is contained in Listing 5.</span></span>

<span data-ttu-id="10f4a-252">**列出 5-Views\Contact\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="10f4a-252">**Listing 5 - Views\Contact\Index.aspx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

<span data-ttu-id="10f4a-253">在列出 5 中，瀏覽器歷程記錄已啟用 pageInit() 函式中。</span><span class="sxs-lookup"><span data-stu-id="10f4a-253">In Listing 5, Browser History is enabled in the pageInit() function.</span></span> <span data-ttu-id="10f4a-254">PageInit() 函式也用於設定巡覽事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-254">The pageInit() function is also used to set up the event handler for the navigate event.</span></span> <span data-ttu-id="10f4a-255">當瀏覽器下一頁或上一頁 按鈕造成的頁面來變更狀態時，會引發導覽事件。</span><span class="sxs-lookup"><span data-stu-id="10f4a-255">The navigate event is raised whenever the browser Forward or Back button causes the state of the page to change.</span></span>

<span data-ttu-id="10f4a-256">當您按一下連絡人群組，稱為 beginContactList() 方法。</span><span class="sxs-lookup"><span data-stu-id="10f4a-256">The beginContactList() method is called when you click a contact group.</span></span> <span data-ttu-id="10f4a-257">這個方法會呼叫 addHistoryPoint() 方法來建立新的記錄點。</span><span class="sxs-lookup"><span data-stu-id="10f4a-257">This method creates a new history point by calling the addHistoryPoint() method.</span></span> <span data-ttu-id="10f4a-258">按一下連絡人群組的識別碼會新增至歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="10f4a-258">The id of the contact group clicked is added to history.</span></span>

<span data-ttu-id="10f4a-259">Expando 屬性連絡人群組連結會從擷取的群組識別碼。</span><span class="sxs-lookup"><span data-stu-id="10f4a-259">The group id is retrieved from an expando attribute on the contact group link.</span></span> <span data-ttu-id="10f4a-260">連結會轉譯下列 Ajax.ActionLink() 呼叫。</span><span class="sxs-lookup"><span data-stu-id="10f4a-260">The link is rendered with the following call to Ajax.ActionLink().</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

<span data-ttu-id="10f4a-261">最後一個參數傳遞至 Ajax.ActionLink() 加入名為 （小寫 XHTML 相容性） 連結的 groupid expando 屬性。</span><span class="sxs-lookup"><span data-stu-id="10f4a-261">The last parameter passed to the Ajax.ActionLink() adds an expando attribute named groupid to the link (lowercase for XHTML compatibility).</span></span>

<span data-ttu-id="10f4a-262">當使用者叫用的瀏覽器上一頁或下一頁 按鈕時，就會引發導覽事件和呼叫 navigate() 方法。</span><span class="sxs-lookup"><span data-stu-id="10f4a-262">When a user hits the browser Back or Forward button, the navigate event is raised and the navigate() method is called.</span></span> <span data-ttu-id="10f4a-263">這個方法會更新以符合對應至瀏覽器歷程記錄點傳遞至瀏覽方法的頁狀態頁面中所顯示的連絡人。</span><span class="sxs-lookup"><span data-stu-id="10f4a-263">This method updates the contacts displayed in the page to match the state of the page that corresponds to the browser history point passed to the navigate method.</span></span>

## <a name="performing-ajax-deletes"></a><span data-ttu-id="10f4a-264">執行 Ajax 刪除</span><span class="sxs-lookup"><span data-stu-id="10f4a-264">Performing Ajax Deletes</span></span>

<span data-ttu-id="10f4a-265">目前，若要刪除連絡人，您必須在 [刪除] 連結上按一下，然後按一下 [刪除確認] 頁面中顯示的 [刪除] 按鈕 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="10f4a-265">Currently, in order to delete a contact, you need to click on the Delete link and then click the Delete button displayed in the delete confirmation page (see Figure 2).</span></span> <span data-ttu-id="10f4a-266">這看起來很簡單，例如刪除資料庫記錄的要求頁面。</span><span class="sxs-lookup"><span data-stu-id="10f4a-266">This seems like a lot of page requests to do something simple like deleting a database record.</span></span>


<span data-ttu-id="10f4a-267">[![[刪除確認] 頁面](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="10f4a-267">[![The delete confirmation page](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)</span></span>

<span data-ttu-id="10f4a-268">**圖 02**： 刪除確認 頁面 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="10f4a-268">**Figure 02**: The delete confirmation page([Click to view full-size image](iteration-7-add-ajax-functionality-cs/_static/image4.png))</span></span>


<span data-ttu-id="10f4a-269">很容易略過 [刪除確認] 頁面，並直接從索引檢視中刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="10f4a-269">It is tempting to skip the delete confirmation page and delete a contact directly from the Index view.</span></span> <span data-ttu-id="10f4a-270">因為這種方式開啟您的應用程式的安全性漏洞，您應該避免此誘惑。</span><span class="sxs-lookup"><span data-stu-id="10f4a-270">You should avoid this temptation because taking this approach opens your application to security holes.</span></span> <span data-ttu-id="10f4a-271">一般情況下，您不要想要執行 HTTP GET 作業，當叫用動作來修改 web 應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10f4a-271">In general, you don t want to perform an HTTP GET operation when invoking an action that modifies the state of your web application.</span></span> <span data-ttu-id="10f4a-272">在執行 delete 時，您會想要執行 HTTP POST，或最好使用 HTTP DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="10f4a-272">When performing a delete, you want to perform an HTTP POST, or better yet, an HTTP DELETE operation.</span></span>

<span data-ttu-id="10f4a-273">[刪除] 連結會包含在部分 ContactList。</span><span class="sxs-lookup"><span data-stu-id="10f4a-273">The Delete link is contained in the ContactList partial.</span></span> <span data-ttu-id="10f4a-274">部分 ContactList 的更新的版本都包含在程式碼範例 6。</span><span class="sxs-lookup"><span data-stu-id="10f4a-274">An updated version of the ContactList partial is contained in Listing 6.</span></span>

<span data-ttu-id="10f4a-275">**Listing 6 - Views\Contact\ContactList.ascx**</span><span class="sxs-lookup"><span data-stu-id="10f4a-275">**Listing 6 - Views\Contact\ContactList.ascx**</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

<span data-ttu-id="10f4a-276">[刪除] 連結會轉譯下列 Ajax.ImageActionLink() 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="10f4a-276">The Delete link is rendered with the following call to the Ajax.ImageActionLink() method:</span></span>

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> <span data-ttu-id="10f4a-277">Ajax.ImageActionLink() 不是 ASP.NET MVC framework 的標準組件。</span><span class="sxs-lookup"><span data-stu-id="10f4a-277">The Ajax.ImageActionLink() is not a standard part of the ASP.NET MVC framework.</span></span> <span data-ttu-id="10f4a-278">Ajax.ImageActionLink() 為連絡人管理員專案中包含自訂的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="10f4a-278">The Ajax.ImageActionLink() is a custom helper methods included in the Contact Manager project.</span></span>


<span data-ttu-id="10f4a-279">AjaxOptions 參數有兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="10f4a-279">The AjaxOptions parameter has two properties.</span></span> <span data-ttu-id="10f4a-280">首先，確認屬性用來顯示快顯 JavaScript 確認對話方塊。</span><span class="sxs-lookup"><span data-stu-id="10f4a-280">First, the Confirm property is used to display a popup JavaScript confirmation dialog.</span></span> <span data-ttu-id="10f4a-281">第二，HttpMethod 屬性用來執行 HTTP DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="10f4a-281">Second, the HttpMethod property is used to perform an HTTP DELETE operation.</span></span>

<span data-ttu-id="10f4a-282">列出 7 包含已新增至連絡人控制站的新 AjaxDelete() 動作。</span><span class="sxs-lookup"><span data-stu-id="10f4a-282">Listing 7 contains a new AjaxDelete() action that has been added to the Contact controller.</span></span>

<span data-ttu-id="10f4a-283">**Listing 7 - Controllers\ContactController.cs (AjaxDelete)**</span><span class="sxs-lookup"><span data-stu-id="10f4a-283">**Listing 7 - Controllers\ContactController.cs (AjaxDelete)**</span></span>

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

<span data-ttu-id="10f4a-284">AjaxDelete() 動作是使用 AcceptVerbs 屬性加以裝飾。</span><span class="sxs-lookup"><span data-stu-id="10f4a-284">The AjaxDelete() action is decorated with an AcceptVerbs attribute.</span></span> <span data-ttu-id="10f4a-285">這個屬性會阻止這個動作，除了 HTTP DELETE 作業以外的任何 HTTP 作業叫用。</span><span class="sxs-lookup"><span data-stu-id="10f4a-285">This attribute prevents the action from being invoked except by any HTTP operation other than an HTTP DELETE operation.</span></span> <span data-ttu-id="10f4a-286">特別是，您無法叫用此動作與 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="10f4a-286">In particular, you cannot invoke this action with an HTTP GET.</span></span>

<span data-ttu-id="10f4a-287">刪除資料庫記錄之後，您需要顯示更新的連絡人清單不包含已刪除的記錄。</span><span class="sxs-lookup"><span data-stu-id="10f4a-287">After you delete database record, you need to display the updated list of contacts that does not contain the deleted record.</span></span> <span data-ttu-id="10f4a-288">AjaxDelete() 方法會傳回部分 ContactList 和更新的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-288">The AjaxDelete() method returns the ContactList partial and the updated list of contacts.</span></span>

## <a name="summary"></a><span data-ttu-id="10f4a-289">總結</span><span class="sxs-lookup"><span data-stu-id="10f4a-289">Summary</span></span>

<span data-ttu-id="10f4a-290">在這個反覆項目，我們加入 Ajax 功能連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="10f4a-290">In this iteration, we added Ajax functionality to our Contact Manager application.</span></span> <span data-ttu-id="10f4a-291">我們使用 Ajax 改善回應性和我們的應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="10f4a-291">We used Ajax to improve the responsiveness and performance of our application.</span></span>

<span data-ttu-id="10f4a-292">首先，我們重構索引檢視，以便按一下連絡人群組，不會更新整個檢視。</span><span class="sxs-lookup"><span data-stu-id="10f4a-292">First, we refactored the Index view so that clicking a contact group does not update the entire view.</span></span> <span data-ttu-id="10f4a-293">相反地，按一下連絡人群組只會更新連絡人的清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-293">Instead, clicking a contact group only updates the list of contacts.</span></span>

<span data-ttu-id="10f4a-294">接下來，我們使用 jQuery 動畫效果淡出及淡入的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="10f4a-294">Next, we used jQuery animation effects to fade out and fade in the list of contacts.</span></span> <span data-ttu-id="10f4a-295">將動畫加入至 Ajax 應用程式可以用來提供與瀏覽器進度列的對等應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="10f4a-295">Adding animation to an Ajax application can be used to provide users of the application with the equivalent of a browser progress bar.</span></span>

<span data-ttu-id="10f4a-296">我們也會加入至我們的 Ajax 應用程式的瀏覽器歷程記錄的支援。</span><span class="sxs-lookup"><span data-stu-id="10f4a-296">We also added browser history support to our Ajax application.</span></span> <span data-ttu-id="10f4a-297">我們已啟用使用者按一下瀏覽器上一頁及下一頁按鈕來變更索引檢視的狀態。</span><span class="sxs-lookup"><span data-stu-id="10f4a-297">We enabled users to click the browser Back and Forward buttons to change the state of the Index view.</span></span>

<span data-ttu-id="10f4a-298">最後，我們會建立支援 HTTP DELETE 作業的刪除連結。</span><span class="sxs-lookup"><span data-stu-id="10f4a-298">Finally, we created a delete link that supports HTTP DELETE operations.</span></span> <span data-ttu-id="10f4a-299">藉由執行 Ajax 刪除，我們會讓使用者刪除資料庫記錄而不需要使用者要求額外的刪除確認 頁面。</span><span class="sxs-lookup"><span data-stu-id="10f4a-299">By performing Ajax deletes, we enable users to delete database records without requiring the user to request an additional delete confirmation page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10f4a-300">[上一頁](iteration-6-use-test-driven-development-cs.md)
> [下一頁](iteration-1-create-the-application-vb.md)</span><span class="sxs-lookup"><span data-stu-id="10f4a-300">[Previous](iteration-6-use-test-driven-development-cs.md)
[Next](iteration-1-create-the-application-vb.md)</span></span>
