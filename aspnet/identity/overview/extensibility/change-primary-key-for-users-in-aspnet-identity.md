---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: 變更主索引鍵中 ASP.NET 識別的使用者 |Microsoft 文件
author: tfitzmac
description: 在 Visual Studio 2013 中，預設 web 應用程式會使用使用者帳戶的金鑰字串值。 ASP.NET 識別可讓您變更的類型...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "26498227"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="a6487-104">在 ASP.NET Identity 中的使用者變更主索引鍵</span><span class="sxs-lookup"><span data-stu-id="a6487-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="a6487-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a6487-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a6487-106">在 Visual Studio 2013 中，預設 web 應用程式會使用使用者帳戶的金鑰字串值。</span><span class="sxs-lookup"><span data-stu-id="a6487-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="a6487-107">ASP.NET 識別可讓您變更以符合資料需求索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="a6487-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="a6487-108">例如，您可以從字串變更索引鍵的類型為整數。</span><span class="sxs-lookup"><span data-stu-id="a6487-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="a6487-109">本主題說明如何開始使用預設 web 應用程式，並將使用者帳戶金鑰變更為整數。</span><span class="sxs-lookup"><span data-stu-id="a6487-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="a6487-110">您可以使用相同的修改，在您的專案中實作任何類型的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a6487-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="a6487-111">它示範如何進行這些變更在預設 web 應用程式，但您無法套用類似的自訂應用程式的修改。</span><span class="sxs-lookup"><span data-stu-id="a6487-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="a6487-112">它會顯示使用 MVC 或 Web Form 時所需的變更。</span><span class="sxs-lookup"><span data-stu-id="a6487-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a6487-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="a6487-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a6487-114">Visual Studio 2013 Update 2 （或更新版本）</span><span class="sxs-lookup"><span data-stu-id="a6487-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="a6487-115">ASP.NET Identity 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a6487-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="a6487-116">若要執行本教學課程步驟，您必須有 Visual Studio 2013 Update 2 （或更新版本） 和 ASP.NET Web 應用程式範本所建立的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6487-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="a6487-117">在 Update 3 中變更範本。</span><span class="sxs-lookup"><span data-stu-id="a6487-117">The template changed in Update 3.</span></span> <span data-ttu-id="a6487-118">本主題說明如何變更更新 2 和 Update 3 中的範本。</span><span class="sxs-lookup"><span data-stu-id="a6487-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="a6487-119">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="a6487-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="a6487-120">變更中識別的使用者類別的索引鍵的類型</span><span class="sxs-lookup"><span data-stu-id="a6487-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="a6487-121">新增自訂身分識別類別，可使用的金鑰類型</span><span class="sxs-lookup"><span data-stu-id="a6487-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="a6487-122">變更要使用的金鑰類型的內容類別及使用者管理員</span><span class="sxs-lookup"><span data-stu-id="a6487-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="a6487-123">變更啟動設定来使用的金鑰類型</span><span class="sxs-lookup"><span data-stu-id="a6487-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="a6487-124">MVC Update 2 中，變更將索引鍵的型別傳遞 AccountController</span><span class="sxs-lookup"><span data-stu-id="a6487-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="a6487-125">MVC 中使用 Update 3，變更 AccountController 以及 ManageController 傳遞的索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="a6487-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="a6487-126">Web Form Update 2，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="a6487-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="a6487-127">Web Form 的 Update 3，將變更傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="a6487-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="a6487-128">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a6487-128">Run application</span></span>](#run)
- [<span data-ttu-id="a6487-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="a6487-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="a6487-130">變更中識別的使用者類別的索引鍵的類型</span><span class="sxs-lookup"><span data-stu-id="a6487-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="a6487-131">在 ASP.NET Web 應用程式範本所建立的專案中，指定 ApplicationUser 類別使用整數之金鑰的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6487-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="a6487-132">在 IdentityModels.cs，變更 ApplicationUser 類別繼承自具有一種 IdentityUser **int** TKey 泛型參數。</span><span class="sxs-lookup"><span data-stu-id="a6487-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="a6487-133">您也會傳遞三個自訂類別尚未不實作它的名稱。</span><span class="sxs-lookup"><span data-stu-id="a6487-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="a6487-134">您已變更的金鑰類型，但根據預設，應用程式的其餘部分仍假設索引鍵是字串。</span><span class="sxs-lookup"><span data-stu-id="a6487-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="a6487-135">您必須明確指示會假設字串的程式碼中的索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="a6487-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="a6487-136">在**ApplicationUser**類別中，變更**GenerateUserIdentityAsync**方法，將包含 int、 反白顯示的下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="a6487-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="a6487-137">這項變更不需要的 Web Form Update 3 範本專案。</span><span class="sxs-lookup"><span data-stu-id="a6487-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="a6487-138">新增自訂身分識別類別，可使用的金鑰類型</span><span class="sxs-lookup"><span data-stu-id="a6487-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="a6487-139">其他身分識別類別，例如 IdentityUserRole、 IdentityUserClaim、 IdentityUserLogin、 IdentityRole、 UserStore、 RoleStore，仍會使用字串索引鍵設定。</span><span class="sxs-lookup"><span data-stu-id="a6487-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="a6487-140">建立指定索引鍵的整數這些類別的新的版本。</span><span class="sxs-lookup"><span data-stu-id="a6487-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="a6487-141">您不需要提供太多的實作程式碼，這些類別中，您主要是只做為索引鍵設定 int。</span><span class="sxs-lookup"><span data-stu-id="a6487-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="a6487-142">將下列類別加入至 IdentityModels.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6487-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="a6487-143">變更要使用的金鑰類型的內容類別及使用者管理員</span><span class="sxs-lookup"><span data-stu-id="a6487-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="a6487-144">在 IdentityModels.cs，變更的定義**ApplicationDbContext**類別，以使用您的新自訂類別和**int**的索引鍵，反白顯示的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="a6487-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="a6487-145">ThrowIfV1Schema 參數不再有效的建構函式中。</span><span class="sxs-lookup"><span data-stu-id="a6487-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="a6487-146">變更建構函式，讓它未通過 ThrowIfV1Schema 值。</span><span class="sxs-lookup"><span data-stu-id="a6487-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="a6487-147">開啟 IdentityConfig.cs，並將變更**ApplicationUserManger**類別，以使用新的使用者儲存的保存資料的類別和**int**索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a6487-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="a6487-148">在 Update 3 範本中，您必須變更 ApplicationSignInManager 類別。</span><span class="sxs-lookup"><span data-stu-id="a6487-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="a6487-149">變更啟動設定来使用的金鑰類型</span><span class="sxs-lookup"><span data-stu-id="a6487-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="a6487-150">在 Startup.Auth.cs，取代 OnValidateIdentity 程式碼，以反白顯示下面。</span><span class="sxs-lookup"><span data-stu-id="a6487-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="a6487-151">請注意 getUserIdCallback 定義中，將字串值剖析為整數。</span><span class="sxs-lookup"><span data-stu-id="a6487-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="a6487-152">如果您的專案無法辨識的泛型實作**GetUserId**方法，您可能需要更新到 2.1 版的 ASP.NET Identity NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="a6487-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="a6487-153">您對進行許多變更 ASP.NET Identity 所使用的基礎結構類別。</span><span class="sxs-lookup"><span data-stu-id="a6487-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="a6487-154">如果您嘗試編譯專案，您會發現許多錯誤。</span><span class="sxs-lookup"><span data-stu-id="a6487-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="a6487-155">幸運的是，其他錯誤都有相似。</span><span class="sxs-lookup"><span data-stu-id="a6487-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="a6487-156">識別類別必須要有整數索引鍵，但控制器 （或 Web Form） 傳遞的字串值。</span><span class="sxs-lookup"><span data-stu-id="a6487-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="a6487-157">在各案例中，您需要從字串和整數轉換，藉由呼叫**GetUserId&lt;int&gt;**。</span><span class="sxs-lookup"><span data-stu-id="a6487-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="a6487-158">您可以逐步完成從編譯錯誤清單，或請遵循以下的變更。</span><span class="sxs-lookup"><span data-stu-id="a6487-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="a6487-159">剩餘的變更取決於您所建立，而且您已安裝 Visual Studio 中的哪個更新專案的類型。</span><span class="sxs-lookup"><span data-stu-id="a6487-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="a6487-160">您可以直接前往下列連結相關的章節</span><span class="sxs-lookup"><span data-stu-id="a6487-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="a6487-161">MVC Update 2 中，變更將索引鍵的型別傳遞 AccountController</span><span class="sxs-lookup"><span data-stu-id="a6487-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="a6487-162">MVC 中使用 Update 3，變更 AccountController 以及 ManageController 傳遞的索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="a6487-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="a6487-163">Web Form Update 2，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="a6487-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="a6487-164">Web Form 的 Update 3，將變更傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="a6487-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="a6487-165">MVC Update 2 中，變更將索引鍵的型別傳遞 AccountController</span><span class="sxs-lookup"><span data-stu-id="a6487-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="a6487-166">開啟 AccountController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6487-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="a6487-167">您需要變更下列方法。</span><span class="sxs-lookup"><span data-stu-id="a6487-167">You need to change the following methods.</span></span>

<span data-ttu-id="a6487-168">**ConfirmEmail**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="a6487-169">**取消關聯**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="a6487-170">**Manage(ManageUserViewModel)** 方法</span><span class="sxs-lookup"><span data-stu-id="a6487-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="a6487-171">**LinkLoginCallback**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="a6487-172">**RemoveAccountList**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="a6487-173">**HasPassword**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="a6487-174">您現在可以[執行應用程式](#run)並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6487-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="a6487-175">MVC 中使用 Update 3，變更 AccountController 以及 ManageController 傳遞的索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="a6487-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="a6487-176">開啟 AccountController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6487-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="a6487-177">您需要變更以下的方法。</span><span class="sxs-lookup"><span data-stu-id="a6487-177">You need to change the following method.</span></span>

<span data-ttu-id="a6487-178">**ConfirmEmail**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="a6487-179">**SendCode**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="a6487-180">開啟 ManageController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6487-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="a6487-181">您需要變更下列方法。</span><span class="sxs-lookup"><span data-stu-id="a6487-181">You need to change the following methods.</span></span>

<span data-ttu-id="a6487-182">**索引**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="a6487-183">**RemoveLogin**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="a6487-184">**AddPhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="a6487-185">**EnableTwoFactorAuthentication**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="a6487-186">**DisableTwoFactorAuthentication**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="a6487-187">**VerifyPhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="a6487-188">**RemovePhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="a6487-189">**ChangePassword**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="a6487-190">**SetPassword**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="a6487-191">**ManageLogins**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="a6487-192">**LinkLoginCallback**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="a6487-193">**HasPassword**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="a6487-194">**HasPhoneNumber**方法</span><span class="sxs-lookup"><span data-stu-id="a6487-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="a6487-195">您現在可以[執行應用程式](#run)並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6487-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="a6487-196">Web Form Update 2，變更要傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="a6487-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="a6487-197">Web Form Update 2，您要變更之下列頁面。</span><span class="sxs-lookup"><span data-stu-id="a6487-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="a6487-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="a6487-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="a6487-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="a6487-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="a6487-201">您現在可以[執行應用程式](#run)並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6487-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="a6487-202">Web Form 的 Update 3，將變更傳遞的索引鍵類型的帳戶頁面</span><span class="sxs-lookup"><span data-stu-id="a6487-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="a6487-203">Web Form 的 Update 3，您要變更之下列頁面。</span><span class="sxs-lookup"><span data-stu-id="a6487-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="a6487-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="a6487-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="a6487-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="a6487-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="a6487-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="a6487-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="a6487-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="a6487-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="a6487-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a6487-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="a6487-212">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a6487-212">Run application</span></span>

<span data-ttu-id="a6487-213">您已完成所有的預設 Web 應用程式範本所需的變更。</span><span class="sxs-lookup"><span data-stu-id="a6487-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="a6487-214">執行應用程式並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6487-214">Run the application and register a new user.</span></span> <span data-ttu-id="a6487-215">註冊使用者之後, 您會發現 AspNetUsers 資料表有整數的識別碼資料行。</span><span class="sxs-lookup"><span data-stu-id="a6487-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![新的主要金鑰](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="a6487-217">如果您先前已經建立 ASP.NET 識別的資料表具有不同主索引鍵，您需要進行一些額外的變更。</span><span class="sxs-lookup"><span data-stu-id="a6487-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="a6487-218">可能的話，只要刪除現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a6487-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="a6487-219">當您執行 web 應用程式，並加入新的使用者，會以正確的設計重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="a6487-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="a6487-220">如果刪除，則不可能執行 code first 移轉，若要變更的資料表。</span><span class="sxs-lookup"><span data-stu-id="a6487-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="a6487-221">不過，新的整數的主索引鍵將不設定為資料庫中的 SQL 識別屬性。</span><span class="sxs-lookup"><span data-stu-id="a6487-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="a6487-222">您必須手動設定的識別碼資料行，做為識別。</span><span class="sxs-lookup"><span data-stu-id="a6487-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="a6487-223">其他資源</span><span class="sxs-lookup"><span data-stu-id="a6487-223">Other resources</span></span>

- [<span data-ttu-id="a6487-224">ASP.NET Identity 的自訂儲存體提供者概觀</span><span class="sxs-lookup"><span data-stu-id="a6487-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="a6487-225">將現有的網站從 SQL 成員資格移轉至 ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="a6487-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="a6487-226">成員資格和 ASP.NET 識別的使用者設定檔通用的提供者資料移轉</span><span class="sxs-lookup"><span data-stu-id="a6487-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="a6487-227">[範例應用程式](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)已變更的主索引鍵</span><span class="sxs-lookup"><span data-stu-id="a6487-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
