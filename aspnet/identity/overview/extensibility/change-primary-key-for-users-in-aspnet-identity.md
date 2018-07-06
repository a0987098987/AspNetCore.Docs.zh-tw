---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: 變更 ASP.NET Identity 中的使用者的主索引鍵 |Microsoft Docs
author: tfitzmac
description: 在 Visual Studio 2013 中，預設的 web 應用程式會使用使用者帳戶的金鑰字串值。 ASP.NET 身分識別可讓您變更的類型...
ms.author: aspnetcontent
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 688a0dc09297a63bcf510aae9c25f303a5627df7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808038"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="45fa5-104">變更 ASP.NET Identity 中的使用者的主索引鍵</span><span class="sxs-lookup"><span data-stu-id="45fa5-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="45fa5-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="45fa5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="45fa5-106">在 Visual Studio 2013 中，預設的 web 應用程式會使用使用者帳戶的金鑰字串值。</span><span class="sxs-lookup"><span data-stu-id="45fa5-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="45fa5-107">ASP.NET 身分識別可讓您變更以符合您資料需求的索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="45fa5-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="45fa5-108">例如，您可以從字串變更索引鍵的類型為整數。</span><span class="sxs-lookup"><span data-stu-id="45fa5-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="45fa5-109">本主題說明如何開始使用預設 web 應用程式，並將使用者帳戶金鑰變更為整數。</span><span class="sxs-lookup"><span data-stu-id="45fa5-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="45fa5-110">您可以使用相同的修改來實作您的專案中的任何類型的金鑰。</span><span class="sxs-lookup"><span data-stu-id="45fa5-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="45fa5-111">它示範如何在預設 web 應用程式中進行這些變更，但您可以套用類似修改自訂的應用程式。</span><span class="sxs-lookup"><span data-stu-id="45fa5-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="45fa5-112">它會顯示使用 MVC 或 Web Form 時所需的變更。</span><span class="sxs-lookup"><span data-stu-id="45fa5-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="45fa5-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="45fa5-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="45fa5-114">Visual Studio 2013 Update 2 （或更新版本）</span><span class="sxs-lookup"><span data-stu-id="45fa5-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="45fa5-115">2.1 或更新版本的 ASP.NET 身分識別</span><span class="sxs-lookup"><span data-stu-id="45fa5-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="45fa5-116">若要執行本教學課程步驟，您必須有 Visual Studio 2013 Update 2 （或更新版本） 和 ASP.NET Web 應用程式範本所建立的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="45fa5-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="45fa5-117">在 Update 3 中變更範本。</span><span class="sxs-lookup"><span data-stu-id="45fa5-117">The template changed in Update 3.</span></span> <span data-ttu-id="45fa5-118">本主題說明如何變更 Update 2 和 Update 3 中的範本。</span><span class="sxs-lookup"><span data-stu-id="45fa5-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="45fa5-119">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="45fa5-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="45fa5-120">變更識別使用者類別中的索引鍵的類型</span><span class="sxs-lookup"><span data-stu-id="45fa5-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="45fa5-121">新增使用索引鍵類型的自訂身分識別類別</span><span class="sxs-lookup"><span data-stu-id="45fa5-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="45fa5-122">變更要使用的金鑰類型的內容類別和使用者管理員</span><span class="sxs-lookup"><span data-stu-id="45fa5-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="45fa5-123">若要使用的金鑰類型的變更啟動設定</span><span class="sxs-lookup"><span data-stu-id="45fa5-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="45fa5-124">Update 2 的 mvc，變更將索引鍵的型別傳遞 AccountController</span><span class="sxs-lookup"><span data-stu-id="45fa5-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="45fa5-125">Update 3 的 mvc，變更 AccountController 和 ManageController 傳遞的索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="45fa5-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="45fa5-126">對於 Update 2 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="45fa5-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="45fa5-127">對於含 Update 3 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="45fa5-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="45fa5-128">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="45fa5-128">Run application</span></span>](#run)
- [<span data-ttu-id="45fa5-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="45fa5-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="45fa5-130">變更識別使用者類別中的索引鍵的類型</span><span class="sxs-lookup"><span data-stu-id="45fa5-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="45fa5-131">在 ASP.NET Web 應用程式範本所建立的專案中，指定 ApplicationUser 類別，會針對使用者帳戶的索引鍵使用整數。</span><span class="sxs-lookup"><span data-stu-id="45fa5-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="45fa5-132">在 IdentityModels.cs，變更 ApplicationUser 類別繼承自具有一種 IdentityUser **int** TKey 泛型參數。</span><span class="sxs-lookup"><span data-stu-id="45fa5-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="45fa5-133">您也會傳遞您有尚未實作三個自訂類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="45fa5-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="45fa5-134">您已變更的索引鍵的類型，但根據預設，應用程式的其餘部分仍假設金鑰是字串。</span><span class="sxs-lookup"><span data-stu-id="45fa5-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="45fa5-135">您必須明確指示會假設字串的程式碼中的索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="45fa5-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="45fa5-136">在  **ApplicationUser**類別中變更**GenerateUserIdentityAsync**方法以包含整數，如下列醒目提示的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="45fa5-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="45fa5-137">這項變更不需要 Web Form 專案，使用 Update 3 範本。</span><span class="sxs-lookup"><span data-stu-id="45fa5-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="45fa5-138">新增使用索引鍵類型的自訂身分識別類別</span><span class="sxs-lookup"><span data-stu-id="45fa5-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="45fa5-139">其他身分識別類別，例如 IdentityUserRole、 IdentityUserClaim、 IdentityUserLogin、 IdentityRole、 UserStore、 RoleStore，都仍會設定為使用字串索引鍵中。</span><span class="sxs-lookup"><span data-stu-id="45fa5-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="45fa5-140">建立新的版本，這些類別的指定索引鍵的整數。</span><span class="sxs-lookup"><span data-stu-id="45fa5-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="45fa5-141">您不需要提供太多的實作程式碼，這些類別中，您主要只要設定 int 當做索引鍵。</span><span class="sxs-lookup"><span data-stu-id="45fa5-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="45fa5-142">將下列類別加入 IdentityModels.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="45fa5-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="45fa5-143">變更要使用的金鑰類型的內容類別和使用者管理員</span><span class="sxs-lookup"><span data-stu-id="45fa5-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="45fa5-144">在 IdentityModels.cs，變更其定義**ApplicationDbContext**類別，以使用您的新自訂類別和**int**針對索引鍵，反白顯示的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="45fa5-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="45fa5-145">ThrowIfV1Schema 參數不再有效的建構函式。</span><span class="sxs-lookup"><span data-stu-id="45fa5-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="45fa5-146">變更建構函式，因此它未通過 ThrowIfV1Schema 值。</span><span class="sxs-lookup"><span data-stu-id="45fa5-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="45fa5-147">開啟 IdentityConfig.cs，並將變更**ApplicationUserManger**類別，以使用新的使用者儲存保存資料的類別和**int**索引鍵。</span><span class="sxs-lookup"><span data-stu-id="45fa5-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="45fa5-148">在 Update 3 範本中，您必須變更 ApplicationSignInManager 類別。</span><span class="sxs-lookup"><span data-stu-id="45fa5-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="45fa5-149">若要使用的金鑰類型的變更啟動設定</span><span class="sxs-lookup"><span data-stu-id="45fa5-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="45fa5-150">在 Startup.Auth.cs，取代 OnValidateIdentity 程式碼，以反白顯示如下。</span><span class="sxs-lookup"><span data-stu-id="45fa5-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="45fa5-151">請注意，getUserIdCallback 定義中，會將字串值剖析成整數。</span><span class="sxs-lookup"><span data-stu-id="45fa5-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="45fa5-152">如果您的專案無法辨識的泛型實作**GetUserId**方法中，您可能需要的 ASP.NET 身分識別的 NuGet 套件更新為版本 2.1</span><span class="sxs-lookup"><span data-stu-id="45fa5-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="45fa5-153">您已進行許多變更以使用 ASP.NET Identity 的基礎結構類別。</span><span class="sxs-lookup"><span data-stu-id="45fa5-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="45fa5-154">如果您嘗試編譯專案，您會發現許多錯誤。</span><span class="sxs-lookup"><span data-stu-id="45fa5-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="45fa5-155">幸運的是，剩餘的錯誤都有相似。</span><span class="sxs-lookup"><span data-stu-id="45fa5-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="45fa5-156">身分識別類別需要整數索引鍵，但控制器 （或 Web Form） 傳遞的字串值。</span><span class="sxs-lookup"><span data-stu-id="45fa5-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="45fa5-157">在每個案例中，您需要將呼叫轉換字串與整數**GetUserId&lt;int&gt;**。</span><span class="sxs-lookup"><span data-stu-id="45fa5-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="45fa5-158">您可以逐步完成從編譯錯誤清單，或遵循下列變更。</span><span class="sxs-lookup"><span data-stu-id="45fa5-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="45fa5-159">剩餘的變更取決於您所建立，而且您已安裝哪些更新 Visual Studio 中的專案類型。</span><span class="sxs-lookup"><span data-stu-id="45fa5-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="45fa5-160">您可以直接跳到相關的區段，透過下列連結</span><span class="sxs-lookup"><span data-stu-id="45fa5-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="45fa5-161">Update 2 的 mvc，變更將索引鍵的型別傳遞 AccountController</span><span class="sxs-lookup"><span data-stu-id="45fa5-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="45fa5-162">Update 3 的 mvc，變更 AccountController 和 ManageController 傳遞的索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="45fa5-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="45fa5-163">對於 Update 2 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="45fa5-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="45fa5-164">對於含 Update 3 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="45fa5-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="45fa5-165">Update 2 的 mvc，變更將索引鍵的型別傳遞 AccountController</span><span class="sxs-lookup"><span data-stu-id="45fa5-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="45fa5-166">開啟 AccountController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="45fa5-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="45fa5-167">您需要變更下列方法。</span><span class="sxs-lookup"><span data-stu-id="45fa5-167">You need to change the following methods.</span></span>

<span data-ttu-id="45fa5-168">**ConfirmEmail**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="45fa5-169">**取消關聯**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="45fa5-170">**Manage(ManageUserViewModel)** 方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="45fa5-171">**LinkLoginCallback**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="45fa5-172">**RemoveAccountList**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="45fa5-173">**HasPassword**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="45fa5-174">您現在可以[執行應用程式](#run)並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="45fa5-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="45fa5-175">Update 3 的 mvc，變更 AccountController 和 ManageController 傳遞的索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="45fa5-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="45fa5-176">開啟 AccountController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="45fa5-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="45fa5-177">您需要將下列方法變更。</span><span class="sxs-lookup"><span data-stu-id="45fa5-177">You need to change the following method.</span></span>

<span data-ttu-id="45fa5-178">**ConfirmEmail**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="45fa5-179">**SendCode**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="45fa5-180">開啟 ManageController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="45fa5-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="45fa5-181">您需要變更下列方法。</span><span class="sxs-lookup"><span data-stu-id="45fa5-181">You need to change the following methods.</span></span>

<span data-ttu-id="45fa5-182">**索引**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="45fa5-183">**RemoveLogin**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="45fa5-184">**AddPhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="45fa5-185">**EnableTwoFactorAuthentication**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="45fa5-186">**DisableTwoFactorAuthentication**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="45fa5-187">**VerifyPhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="45fa5-188">**RemovePhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="45fa5-189">**ChangePassword**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="45fa5-190">**SetPassword**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="45fa5-191">**ManageLogins**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="45fa5-192">**LinkLoginCallback**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="45fa5-193">**HasPassword**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="45fa5-194">**HasPhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="45fa5-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="45fa5-195">您現在可以[執行應用程式](#run)並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="45fa5-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="45fa5-196">對於 Update 2 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="45fa5-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="45fa5-197">對於 Update 2 的 Web Form，您需要變更下列頁面。</span><span class="sxs-lookup"><span data-stu-id="45fa5-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="45fa5-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="45fa5-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="45fa5-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="45fa5-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="45fa5-201">您現在可以[執行應用程式](#run)並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="45fa5-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="45fa5-202">對於含 Update 3 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="45fa5-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="45fa5-203">對於 Web Form Update 3，您需要變更下列頁面。</span><span class="sxs-lookup"><span data-stu-id="45fa5-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="45fa5-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="45fa5-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="45fa5-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="45fa5-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="45fa5-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="45fa5-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="45fa5-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="45fa5-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="45fa5-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="45fa5-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="45fa5-212">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="45fa5-212">Run application</span></span>

<span data-ttu-id="45fa5-213">您已完成所有必要變更預設的 Web 應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="45fa5-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="45fa5-214">執行應用程式並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="45fa5-214">Run the application and register a new user.</span></span> <span data-ttu-id="45fa5-215">註冊使用者之後, 您會發現 AspNetUsers 資料表已經是一個整數的識別碼資料行。</span><span class="sxs-lookup"><span data-stu-id="45fa5-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![新的主要金鑰](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="45fa5-217">如果您先前建立 ASP.NET 身分識別的資料表以不同的主索引鍵，您需要進行一些額外的變更。</span><span class="sxs-lookup"><span data-stu-id="45fa5-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="45fa5-218">可能的話，只要刪除現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="45fa5-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="45fa5-219">當您執行 web 應用程式，並新增使用者時，會以正確的設計重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="45fa5-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="45fa5-220">如果刪除是不可能的話，請執行 code first 移轉來變更資料表。</span><span class="sxs-lookup"><span data-stu-id="45fa5-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="45fa5-221">不過，新的整數主索引鍵將不會設定為在資料庫中的 SQL 識別屬性。</span><span class="sxs-lookup"><span data-stu-id="45fa5-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="45fa5-222">您必須手動設定的識別碼資料行，做為識別。</span><span class="sxs-lookup"><span data-stu-id="45fa5-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="45fa5-223">其他資源</span><span class="sxs-lookup"><span data-stu-id="45fa5-223">Other resources</span></span>

- [<span data-ttu-id="45fa5-224">ASP.NET Identity 的自訂儲存體提供者概觀</span><span class="sxs-lookup"><span data-stu-id="45fa5-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="45fa5-225">將現有的網站從 SQL 成員資格移轉至 ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="45fa5-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="45fa5-226">成員資格和使用者設定檔，以 ASP.NET 身分識別的通用提供者資料移轉</span><span class="sxs-lookup"><span data-stu-id="45fa5-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="45fa5-227">[範例應用程式](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)已變更的主索引鍵</span><span class="sxs-lookup"><span data-stu-id="45fa5-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
