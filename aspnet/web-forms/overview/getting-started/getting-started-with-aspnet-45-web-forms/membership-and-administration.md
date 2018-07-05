---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: 成員資格和管理 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 58eda260ccf2d0f237aaade65017760ed87a8c4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368294"
---
<a name="membership-and-administration"></a><span data-ttu-id="638a7-103">成員資格和管理</span><span class="sxs-lookup"><span data-stu-id="638a7-103">Membership and Administration</span></span>
====================
<span data-ttu-id="638a7-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="638a7-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="638a7-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="638a7-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="638a7-106">本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="638a7-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="638a7-107">Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="638a7-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="638a7-108">本教學課程會示範如何更新 Wingtip Toys 範例應用程式，來新增自訂角色，並使用 ASP.NET 身分識別。</span><span class="sxs-lookup"><span data-stu-id="638a7-108">This tutorial shows you how to update the Wingtip Toys sample application to add a custom role and use ASP.NET Identity.</span></span> <span data-ttu-id="638a7-109">它也示範如何實作的自訂角色的使用者可以新增和移除網站中的產品管理頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-109">It also shows you how to implement an administration page from which the user with a custom role can add and remove products from the website.</span></span>

<span data-ttu-id="638a7-110">[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)是用來建置 ASP.NET web 應用程式的成員資格系統，並且使用 ASP.NET 4.5 中。</span><span class="sxs-lookup"><span data-stu-id="638a7-110">[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) is the membership system used to build ASP.NET web application and is available in ASP.NET 4.5.</span></span> <span data-ttu-id="638a7-111">在 Visual Studio 2013 Web Form 專案範本，以及適用於的範本會使用 ASP.NET 身分識別[ASP.NET MVC](../../../../mvc/index.md)， [ASP.NET Web API](../../../../web-api/index.md)，和[ASP.NET 單一頁面應用程式](../../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="638a7-111">ASP.NET Identity is used in the Visual Studio 2013 Web Forms project template, as well as the templates for [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), and [ASP.NET Single Page Application](../../../../single-page-application/index.md).</span></span> <span data-ttu-id="638a7-112">您還會特別可以安裝與空的 Web 應用程式啟動時，使用 NuGet 的 ASP.NET 身分識別系統。</span><span class="sxs-lookup"><span data-stu-id="638a7-112">You can also specifically install the ASP.NET Identity system using NuGet when you start with an empty Web application.</span></span> <span data-ttu-id="638a7-113">不過，在本教學課程系列中您使用**Web Form**projecttemplate，其中包含 ASP.NET 身分識別系統。</span><span class="sxs-lookup"><span data-stu-id="638a7-113">However, in this tutorial series you use the **Web Forms**projecttemplate, which includes the ASP.NET Identity system.</span></span> <span data-ttu-id="638a7-114">ASP.NET 身分識別可讓您更輕鬆地整合應用程式資料的特定使用者設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="638a7-114">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="638a7-115">此外，ASP.NET 身分識別可讓您選擇 您的應用程式中的 使用者設定檔的持續性模型。</span><span class="sxs-lookup"><span data-stu-id="638a7-115">Also, ASP.NET Identity allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="638a7-116">您可以將資料儲存在 SQL Server 資料庫或其他資料存放區，包括*NoSQL*資料存放區，例如 Windows Azure 儲存體資料表。</span><span class="sxs-lookup"><span data-stu-id="638a7-116">You can store the data in a SQL Server database or another data store, including *NoSQL* data stores such as Windows Azure Storage Tables.</span></span>

<span data-ttu-id="638a7-117">本教學課程是根據先前的教學課程，Wingtip Toys 教學課程系列中標題為 「 簽出和付款使用 PayPal"。</span><span class="sxs-lookup"><span data-stu-id="638a7-117">This tutorial builds on the previous tutorial titled "Checkout and Payment with PayPal" in the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="638a7-118">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="638a7-118">What you'll learn:</span></span>

- <span data-ttu-id="638a7-119">如何將自訂的角色和使用者新增至應用程式中使用程式碼。</span><span class="sxs-lookup"><span data-stu-id="638a7-119">How to use code to add a custom role and a user to the application.</span></span>
- <span data-ttu-id="638a7-120">如何限制對管理資料夾，以及頁面存取。</span><span class="sxs-lookup"><span data-stu-id="638a7-120">How to restrict access to the administration folder and page.</span></span>
- <span data-ttu-id="638a7-121">如何提供隸屬於自訂的角色之使用者的瀏覽。</span><span class="sxs-lookup"><span data-stu-id="638a7-121">How to provide navigation for the user that belongs to the custom role.</span></span>
- <span data-ttu-id="638a7-122">如何使用模型繫結來填入[DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)與產品類別目錄的控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-122">How to use model binding to populate a [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) control with product categories.</span></span>
- <span data-ttu-id="638a7-123">如何將檔案上傳至 web 應用程式使用[FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-123">How to upload a file to the web application using the [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) control.</span></span>
- <span data-ttu-id="638a7-124">如何使用驗證控制項來實作輸入的驗證。</span><span class="sxs-lookup"><span data-stu-id="638a7-124">How to use validation controls to implement input validation.</span></span>
- <span data-ttu-id="638a7-125">如何新增和移除應用程式中的產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-125">How to add and remove products from the application.</span></span>

## <a name="these-features-are-included-in-the-tutorial"></a><span data-ttu-id="638a7-126">在本教學課程中包含這些功能：</span><span class="sxs-lookup"><span data-stu-id="638a7-126">These features are included in the tutorial:</span></span>

- <span data-ttu-id="638a7-127">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="638a7-127">ASP.NET Identity</span></span>
- <span data-ttu-id="638a7-128">組態和授權</span><span class="sxs-lookup"><span data-stu-id="638a7-128">Configuration and Authorization</span></span>
- <span data-ttu-id="638a7-129">模型繫結</span><span class="sxs-lookup"><span data-stu-id="638a7-129">Model Binding</span></span>
- <span data-ttu-id="638a7-130">低調驗證</span><span class="sxs-lookup"><span data-stu-id="638a7-130">Unobtrusive Validation</span></span>

<span data-ttu-id="638a7-131">ASP.NET Web Forms 提供成員資格功能。</span><span class="sxs-lookup"><span data-stu-id="638a7-131">ASP.NET Web Forms provides membership capabilities.</span></span> <span data-ttu-id="638a7-132">藉由使用預設範本，您會有內建的成員資格功能，您可以立即使用應用程式執行時。</span><span class="sxs-lookup"><span data-stu-id="638a7-132">By using the default template, you have built-in membership functionality that you can immediately use when the application runs.</span></span> <span data-ttu-id="638a7-133">本教學課程會示範如何使用 ASP.NET 身分識別來加入自訂角色，並將使用者指派給該角色。</span><span class="sxs-lookup"><span data-stu-id="638a7-133">This tutorial shows you how to use ASP.NET Identity to add a custom role and assign a user to that role.</span></span> <span data-ttu-id="638a7-134">您將了解如何限制對管理資料夾的存取。</span><span class="sxs-lookup"><span data-stu-id="638a7-134">You will learn how to restrict access to the administration folder.</span></span> <span data-ttu-id="638a7-135">您將新增頁面可讓使用者使用自訂角色來新增和移除的產品，以及預覽產品之後已新增, 的系統管理 資料夾。</span><span class="sxs-lookup"><span data-stu-id="638a7-135">You'll add a page to the administration folder that allows a user with a custom role to add and remove products, and to preview a product after it has been added.</span></span>

## <a name="adding-a-custom-role"></a><span data-ttu-id="638a7-136">新增自訂角色</span><span class="sxs-lookup"><span data-stu-id="638a7-136">Adding a Custom Role</span></span>

<span data-ttu-id="638a7-137">使用 ASP.NET 身分識別，新增自訂角色，並將使用者指派給該角色使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="638a7-137">Using ASP.NET Identity, you can add a custom role and assign a user to that role using code.</span></span>

1. <span data-ttu-id="638a7-138">在**方案總管**，以滑鼠右鍵按一下*邏輯*資料夾，並建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="638a7-138">In **Solution Explorer**, right-click on the *Logic* folder and create a new class.</span></span>
2. <span data-ttu-id="638a7-139">新類別命名*RoleActions.cs*。</span><span class="sxs-lookup"><span data-stu-id="638a7-139">Name the new class *RoleActions.cs*.</span></span>
3. <span data-ttu-id="638a7-140">修改程式碼，讓它看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="638a7-140">Modify the code so that it appears as follows:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. <span data-ttu-id="638a7-141">在 **方案總管**，開啟*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="638a7-141">In **Solution Explorer**, open the *Global.asax.cs* file.</span></span>
5. <span data-ttu-id="638a7-142">修改*Global.asax.cs*檔案中新增，使其出現，如下所示，以黃色醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="638a7-142">Modify the *Global.asax.cs* file by adding the code highlighted in yellow so that it appears as follows:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. <span data-ttu-id="638a7-143">請注意，`AddUserAndRole`會以紅色加上底線。</span><span class="sxs-lookup"><span data-stu-id="638a7-143">Notice that `AddUserAndRole` is underlined in red.</span></span> <span data-ttu-id="638a7-144">按兩下 AddUserAndRole 程式碼。</span><span class="sxs-lookup"><span data-stu-id="638a7-144">Double-click the AddUserAndRole code.</span></span>  
   <span data-ttu-id="638a7-145">字母"A"（反白顯示方法的開頭會加上底線。</span><span class="sxs-lookup"><span data-stu-id="638a7-145">The letter "A" at the beginning of the highlighted method will be underlined.</span></span>
7. <span data-ttu-id="638a7-146">停留在字母"A"，然後按一下 UI，可讓您產生方法虛設常式`AddUserAndRole`方法。</span><span class="sxs-lookup"><span data-stu-id="638a7-146">Hover over the letter "A" and click the UI that allows you to generate a method stub for the `AddUserAndRole` method.</span></span> 

    ![成員資格和 Advministration-產生方法虛設常式](membership-and-administration/_static/image1.png)
8. <span data-ttu-id="638a7-148">按一下標題為的選項：</span><span class="sxs-lookup"><span data-stu-id="638a7-148">Click the option titled:</span></span>  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. <span data-ttu-id="638a7-149">開啟*RoleActions.cs*檔案*邏輯*資料夾。</span><span class="sxs-lookup"><span data-stu-id="638a7-149">Open the *RoleActions.cs* file from the *Logic* folder.</span></span>  
   <span data-ttu-id="638a7-150">`AddUserAndRole`方法已加入至類別檔案。</span><span class="sxs-lookup"><span data-stu-id="638a7-150">The `AddUserAndRole` method has been added to the class file.</span></span>
10. <span data-ttu-id="638a7-151">修改*RoleActions.cs*檔案，藉由移除`NotImplementedeException`和新增以黃色反白顯示的程式碼，使它看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="638a7-151">Modify the *RoleActions.cs* file by removing the `NotImplementedeException` and adding the code highlighted in yellow, so that it appears as follows:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

<span data-ttu-id="638a7-152">上述程式碼會先建立成員資格資料庫的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="638a7-152">The above code first establishes a database context for the membership database.</span></span> <span data-ttu-id="638a7-153">成員資格資料庫也會儲存成 *.mdf*中的檔案*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="638a7-153">The membership database is also stored as an *.mdf* file in the *App\_Data* folder.</span></span> <span data-ttu-id="638a7-154">您可以檢視此資料庫之後第一位使用者已登入此 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="638a7-154">You will be able to view this database once the first user has signed in to this web application.</span></span> 

> [!NOTE] 
> 
> <span data-ttu-id="638a7-155">如果您想要儲存成員資格資料，以及產品資料，您可以考慮使用相同**DbContext**您用來將產品的資料儲存在上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="638a7-155">If you wish to store the membership data along with the product data, you can consider using the same **DbContext** that you used to store the product data in the above code.</span></span>


 <span data-ttu-id="638a7-156">*內部*關鍵字是類型 （例如類別） 和類型成員 （例如方法或屬性） 的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="638a7-156">The *internal* keyword is an access modifier for types (such as classes) and type members (such as methods or properties).</span></span> <span data-ttu-id="638a7-157">內部類型或成員都只能包含在相同的組件的檔案內存取 *(.dll*檔案)。</span><span class="sxs-lookup"><span data-stu-id="638a7-157">Internal types or members are accessible only within files contained in the same assembly *(.dll* file).</span></span> <span data-ttu-id="638a7-158">當您建置您的應用程式組件檔案 *(.dll*) 會建立包含您執行應用程式時所執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="638a7-158">When you build your application, an assembly file *(.dll*) is created that contains the code that is executed when you run your application.</span></span> 

<span data-ttu-id="638a7-159">A`RoleStore`物件，可提供角色管理，會根據資料庫的內容建立。</span><span class="sxs-lookup"><span data-stu-id="638a7-159">A `RoleStore` object, which provides role management, is created based on the database context.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="638a7-160">請注意，當`RoleStore`會建立物件，它會使用泛型`IdentityRole`型別。</span><span class="sxs-lookup"><span data-stu-id="638a7-160">Notice that when the `RoleStore` object is created it uses a Generic `IdentityRole` type.</span></span> <span data-ttu-id="638a7-161">這表示`RoleStore`只可包含`IdentityRole`物件。</span><span class="sxs-lookup"><span data-stu-id="638a7-161">This means that the `RoleStore` is only allowed to contain `IdentityRole` objects.</span></span> <span data-ttu-id="638a7-162">也藉由使用泛型時，在記憶體中的資源會處理更好。</span><span class="sxs-lookup"><span data-stu-id="638a7-162">Also by using Generics, resources in memory are handled better.</span></span>


<span data-ttu-id="638a7-163">下一步`RoleManager`物件，會根據建立`RoleStore`您剛才建立的物件。</span><span class="sxs-lookup"><span data-stu-id="638a7-163">Next, the `RoleManager` object, is created based on the `RoleStore` object that you just created.</span></span> <span data-ttu-id="638a7-164">`RoleManager`物件會公開角色相關的 API，可用來自動將變更儲存到`RoleStore`。</span><span class="sxs-lookup"><span data-stu-id="638a7-164">the `RoleManager` object exposes role related API which can be used to automatically save changes to the `RoleStore`.</span></span> <span data-ttu-id="638a7-165">`RoleManager`只可包含`IdentityRole`物件，因為程式碼會使用`<IdentityRole>`泛型型別。</span><span class="sxs-lookup"><span data-stu-id="638a7-165">The `RoleManager` is only allowed to contain `IdentityRole` objects because the code uses the `<IdentityRole>` Generic type.</span></span>

<span data-ttu-id="638a7-166">您呼叫`RoleExists`方法，以判斷是否"canEdit"角色成員資格資料庫中存在。</span><span class="sxs-lookup"><span data-stu-id="638a7-166">You call the `RoleExists` method to determine if the "canEdit" role is present in the membership database.</span></span> <span data-ttu-id="638a7-167">如果不是，您會建立角色。</span><span class="sxs-lookup"><span data-stu-id="638a7-167">If it is not, you create the role.</span></span>

<span data-ttu-id="638a7-168">建立`UserManager`物件似乎是單純`RoleManager`控制，不過它是幾乎完全相同。</span><span class="sxs-lookup"><span data-stu-id="638a7-168">Creating the `UserManager` object appears to be more complicated than the `RoleManager` control, however it is nearly the same.</span></span> <span data-ttu-id="638a7-169">它只會在同一行而不是數個編碼。</span><span class="sxs-lookup"><span data-stu-id="638a7-169">It is just coded on one line rather than several.</span></span> <span data-ttu-id="638a7-170">在這裡，您傳遞的參數具現化作為新的物件，包含在括號中。</span><span class="sxs-lookup"><span data-stu-id="638a7-170">Here, the parameter that you are passing is instantiating as a new object contained in the parenthesis.</span></span>

<span data-ttu-id="638a7-171">接下來您建立 「 canEditUser"使用者藉由建立新`ApplicationUser`物件。</span><span class="sxs-lookup"><span data-stu-id="638a7-171">Next you create the "canEditUser" user by creating a new `ApplicationUser` object.</span></span> <span data-ttu-id="638a7-172">然後，如果您已成功建立使用者，您將使用者新增至新的角色。</span><span class="sxs-lookup"><span data-stu-id="638a7-172">Then, if you successfully create the user, you add the user to the new role.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="638a7-173">稍後在本教學課程系列中的 「 ASP.NET 錯誤處理 」 教學課程期間，將會更新錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="638a7-173">The error handling will be updated during the "ASP.NET Error Handling" tutorial later in this tutorial series.</span></span>


<span data-ttu-id="638a7-174">下一次應用程式啟動時，名為"canEditUser 」 的使用者會新增為名為"canEdit"的應用程式的角色。</span><span class="sxs-lookup"><span data-stu-id="638a7-174">The next time the application starts, the user named "canEditUser" will be added as the role named "canEdit" of the application.</span></span> <span data-ttu-id="638a7-175">稍後在本教學課程中，您將使用者身分登入 」 canEditUser 」 以顯示您將在本教學課程期間新增的其他功能。</span><span class="sxs-lookup"><span data-stu-id="638a7-175">Later in this tutorial, you will login as the "canEditUser" user to display additional capabilities that you will added during this tutorial.</span></span> <span data-ttu-id="638a7-176">如需 ASP.NET 身分識別的 API 詳細資訊，請參閱 < [Microsoft.AspNet.Identity 命名空間](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="638a7-176">For API details about ASP.NET Identity, see the [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx).</span></span> <span data-ttu-id="638a7-177">如需初始化 ASP.NET 身分識別系統的詳細資訊，請參閱 < [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)。</span><span class="sxs-lookup"><span data-stu-id="638a7-177">For additional details about initializing the ASP.NET Identity system, see the [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).</span></span>

### <a name="restricting-access-to-the-administration-page"></a><span data-ttu-id="638a7-178">限制存取的管理頁面</span><span class="sxs-lookup"><span data-stu-id="638a7-178">Restricting Access to the Administration Page</span></span>

<span data-ttu-id="638a7-179">Wingtip Toys 範例應用程式可讓匿名使用者和登入的使用者檢視及購買產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-179">The Wingtip Toys sample application allows both anonymous users and logged-in users to view and purchase products.</span></span> <span data-ttu-id="638a7-180">不過，有自訂"canEdit"角色的登入的使用者可以存取受限制的頁面，以新增和移除產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-180">However, the logged-in user that has the custom "canEdit" role can access a restricted page in order to add and remove products.</span></span>

#### <a name="add-an-administration-folder-and-page"></a><span data-ttu-id="638a7-181">新增管理資料夾和頁面</span><span class="sxs-lookup"><span data-stu-id="638a7-181">Add an Administration Folder and Page</span></span>

<span data-ttu-id="638a7-182">接下來，您將建立名為的資料夾*Admin* "canEditUser 」 使用者屬於 Wingtip Toys 的自訂角色的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="638a7-182">Next, you will create a folder named *Admin* for the "canEditUser" user belonging to the custom role of the Wingtip Toys sample application.</span></span>

1. <span data-ttu-id="638a7-183">以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管**，然後選取**新增** - &gt; **新資料夾**.</span><span class="sxs-lookup"><span data-stu-id="638a7-183">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add** -&gt; **New Folder**.</span></span>
2. <span data-ttu-id="638a7-184">將新資料夾命名*Admin*。</span><span class="sxs-lookup"><span data-stu-id="638a7-184">Name the new folder *Admin*.</span></span>
3. <span data-ttu-id="638a7-185">以滑鼠右鍵按一下*系統管理員*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="638a7-185">Right-click the *Admin* folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="638a7-186">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="638a7-186">The **Add New Item** dialog box is displayed.</span></span>
4. <span data-ttu-id="638a7-187">選取  <strong>Visual C#</strong> - &gt; <strong>Web</strong>左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="638a7-187">Select the <strong>Visual C#</strong>-&gt; <strong>Web</strong> templates group on the left.</span></span> <span data-ttu-id="638a7-188">從清單的中間，選取<strong>使用主版頁面的 Web Form</strong>，其命名<em>AdminPage.aspx</em><strong>，</strong> ，然後選取<strong>新增</strong>。</span><span class="sxs-lookup"><span data-stu-id="638a7-188">From the middle list, select <strong>Web Form with Master Page</strong>,name it <em>AdminPage.aspx</em><strong>,</strong> and then select <strong>Add</strong>.</span></span>
5. <span data-ttu-id="638a7-189">選取  *Site.Master*主版頁面中，為檔案，然後再選擇**確定**。</span><span class="sxs-lookup"><span data-stu-id="638a7-189">Select the *Site.Master* file as the master page, and then choose **OK**.</span></span>

#### <a name="add-a-webconfig-file"></a><span data-ttu-id="638a7-190">加入 Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="638a7-190">Add a Web.config File</span></span>

<span data-ttu-id="638a7-191">藉由新增*Web.config*的檔案*管理員*資料夾中，您可以限制存取包含在資料夾中的頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-191">By adding a *Web.config* file to the *Admin* folder, you can restrict access to the page contained in the folder.</span></span>

1. <span data-ttu-id="638a7-192">以滑鼠右鍵按一下*系統管理員*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="638a7-192">Right-click the *Admin* folder and select **Add** -&gt; **New Item**.</span></span>  
   <span data-ttu-id="638a7-193">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="638a7-193">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="638a7-194">從 Visual C# web 範本的清單中，選取<strong>Web 組態檔</strong>從中間清單中，接受預設名稱<em>Web.config</em><strong>，</strong> ，然後選取<strong>新增</strong>。</span><span class="sxs-lookup"><span data-stu-id="638a7-194">From the list of Visual C# web templates, select <strong>Web Configuration File</strong>from the middle list, accept the default name of <em>Web.config</em><strong>,</strong> and then select <strong>Add</strong>.</span></span>
3. <span data-ttu-id="638a7-195">取代現有的 XML 中的內容*Web.config*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="638a7-195">Replace the existing XML content in the *Web.config* file with the following:</span></span>  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

<span data-ttu-id="638a7-196">儲存*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="638a7-196">Save the *Web.config* file.</span></span> <span data-ttu-id="638a7-197">*Web.config*檔案會指定屬於"canEdit"角色的應用程式的唯一使用者可以存取包含在頁面*管理員*資料夾。</span><span class="sxs-lookup"><span data-stu-id="638a7-197">The *Web.config* file specifies that only user belonging to the "canEdit" role of the application can access the page contained in the *Admin* folder.</span></span>

### <a name="including-custom-role-navigation"></a><span data-ttu-id="638a7-198">包括自訂角色瀏覽</span><span class="sxs-lookup"><span data-stu-id="638a7-198">Including Custom Role Navigation</span></span>

<span data-ttu-id="638a7-199">若要啟用自訂"canEdit"角色的使用者，以瀏覽至 [管理] 區段中的應用程式，您必須新增連結*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-199">To enable the user of the custom "canEdit" role to navigate to the administration section of the application, you must add a link to the *Site.Master* page.</span></span> <span data-ttu-id="638a7-200">只有屬於"canEdit"角色的使用者將能夠看到**Admin**連結和存取管理 區段中的色彩。</span><span class="sxs-lookup"><span data-stu-id="638a7-200">Only users that belong to the "canEdit" role will be able to see the **Admin** link and access the administration section.</span></span>

1. <span data-ttu-id="638a7-201">在 [方案總管] 中，尋找並開啟*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-201">In Solution Explorer, find and open the *Site.Master* page.</span></span>
2. <span data-ttu-id="638a7-202">若要建立的連結"canEdit"角色的使用者，加入下列的未排序清單的黃色反白顯示的標記`<ul>`項目，讓清單會顯示為如下：</span><span class="sxs-lookup"><span data-stu-id="638a7-202">To create a link for the user of the "canEdit" role, add the markup highlighted in yellow to the following unordered list `<ul>` element so that the list appears as follows:</span></span>  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. <span data-ttu-id="638a7-203">開啟*Site.Master.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="638a7-203">Open the *Site.Master.cs* file.</span></span> <span data-ttu-id="638a7-204">製作**系統管理員**只加入程式碼中以黃色反白顯示"canEditUser 」 使用者可看見的連結`Page_Load`處理常式。</span><span class="sxs-lookup"><span data-stu-id="638a7-204">Make the **Admin** link visible only to the "canEditUser" user by adding the code highlighted in yellow to the `Page_Load` handler.</span></span> <span data-ttu-id="638a7-205">`Page_Load`處理常式將會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="638a7-205">The `Page_Load` handler will appear as follows:</span></span>   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

<span data-ttu-id="638a7-206">當頁面載入的程式碼會檢查登入的使用者是否具有"canEdit"角色。</span><span class="sxs-lookup"><span data-stu-id="638a7-206">When the page loads, the code checks whether the logged-in user has the role of "canEdit".</span></span> <span data-ttu-id="638a7-207">如果使用者屬於"canEdit"角色，而範圍的項目，包含的連結*AdminPage.aspx*網頁 （且因此是在範圍內連結） 設為可見。</span><span class="sxs-lookup"><span data-stu-id="638a7-207">If the user belongs to the "canEdit" role, the span element containing the link to the *AdminPage.aspx* page (and consequently the link inside the span) is made visible.</span></span>

### <a name="enabling-product-administration"></a><span data-ttu-id="638a7-208">啟用的產品管理</span><span class="sxs-lookup"><span data-stu-id="638a7-208">Enabling Product Administration</span></span>

<span data-ttu-id="638a7-209">到目前為止，您已建立"canEdit"角色，並加入 「 canEditUser 」 使用者、 系統管理 資料夾，以及管理頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-209">So far, you have created the "canEdit" role and added an "canEditUser" user, an administration folder, and an administration page.</span></span> <span data-ttu-id="638a7-210">您已設定 頁面上，與管理資料夾的存取權限和應用程式中新增"canEdit"角色的使用者之瀏覽連結。</span><span class="sxs-lookup"><span data-stu-id="638a7-210">You have set access rights for the administration folder and page, and have added a navigation link for the user of the "canEdit" role to the application.</span></span> <span data-ttu-id="638a7-211">接下來，您會將標記新增至*AdminPage.aspx*頁面上，並以程式碼*AdminPage.aspx.cs*程式碼後置檔案，可讓具有"canEdit"角色的使用者，新增和移除產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-211">Next, you will add markup to the *AdminPage.aspx* page and code to the *AdminPage.aspx.cs* code-behind file that will enable the user with the "canEdit" role to add and remove products.</span></span>

1. <span data-ttu-id="638a7-212">中**方案總管**，開啟*AdminPage.aspx*從檔案*管理員*資料夾。</span><span class="sxs-lookup"><span data-stu-id="638a7-212">In **Solution Explorer**, open the *AdminPage.aspx* file from the *Admin* folder.</span></span>
2. <span data-ttu-id="638a7-213">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="638a7-213">Replace the existing markup with the following:</span></span>  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. <span data-ttu-id="638a7-214">接下來，開啟*AdminPage.aspx.cs*程式碼後置檔案，以滑鼠右鍵按一下*AdminPage.aspx* ，然後按一下**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="638a7-214">Next, open the *AdminPage.aspx.cs* code-behind file by right-clicking the *AdminPage.aspx* and clicking **View Code**.</span></span>
4. <span data-ttu-id="638a7-215">取代現有的程式碼中*AdminPage.aspx.cs*為下列程式碼的程式碼後置檔案：</span><span class="sxs-lookup"><span data-stu-id="638a7-215">Replace the existing code in the *AdminPage.aspx.cs* code-behind file with the following code:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

<span data-ttu-id="638a7-216">在您所輸入的程式碼*AdminPage.aspx.cs*程式碼後置檔案，稱為類別`AddProducts`將產品加入至資料庫的實際的工作。</span><span class="sxs-lookup"><span data-stu-id="638a7-216">In the code that you entered for the *AdminPage.aspx.cs* code-behind file, a class called `AddProducts` does the actual work of adding products to the database.</span></span> <span data-ttu-id="638a7-217">這個類別還不存在，因此會立即建立。</span><span class="sxs-lookup"><span data-stu-id="638a7-217">This class doesn't exist yet, so you will create it now.</span></span>

1. <span data-ttu-id="638a7-218">在 [**方案總管] 中**，以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="638a7-218">In **Solution Explorer**, right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="638a7-219">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="638a7-219">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="638a7-220">選取  **Visual C#**  - &gt; **程式碼**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="638a7-220">Select the **Visual C#** -&gt; **Code** templates group on the left.</span></span> <span data-ttu-id="638a7-221">然後，選取**類別**從中間清單並將它命名*AddProducts.cs*。</span><span class="sxs-lookup"><span data-stu-id="638a7-221">Then, select **Class**from the middle list and name it *AddProducts.cs*.</span></span>   
   <span data-ttu-id="638a7-222">會顯示新的類別檔案。</span><span class="sxs-lookup"><span data-stu-id="638a7-222">The new class file is displayed.</span></span>
3. <span data-ttu-id="638a7-223">將現有的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="638a7-223">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

<span data-ttu-id="638a7-224">*AdminPage.aspx*  頁面可讓屬於"canEdit"角色的使用者，新增和移除產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-224">The *AdminPage.aspx* page allows the user belonging to the "canEdit" role to add and remove products.</span></span> <span data-ttu-id="638a7-225">當加入新的產品時，產品的相關詳細資料已經過驗證，並接著將其輸入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="638a7-225">When a new product is added, the details about the product are validated and then entered into the database.</span></span> <span data-ttu-id="638a7-226">Web 應用程式的所有使用者立即使用新的產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-226">The new product is immediately available to all users of the web application.</span></span>

#### <a name="unobtrusive-validation"></a><span data-ttu-id="638a7-227">低調驗證</span><span class="sxs-lookup"><span data-stu-id="638a7-227">Unobtrusive Validation</span></span>

<span data-ttu-id="638a7-228">使用者提供的產品詳細資料*AdminPage.aspx*頁面會使用驗證控制項進行驗證 (`RequiredFieldValidator`和`RegularExpressionValidator`)。</span><span class="sxs-lookup"><span data-stu-id="638a7-228">The product details that the user provides on the *AdminPage.aspx* page are validated using validation controls (`RequiredFieldValidator` and `RegularExpressionValidator`).</span></span> <span data-ttu-id="638a7-229">這些控制項，會自動使用低調驗證。</span><span class="sxs-lookup"><span data-stu-id="638a7-229">These controls automatically use unobtrusive validation.</span></span> <span data-ttu-id="638a7-230">低調驗證可讓使用 JavaScript 的用戶端驗證邏輯，這表示頁面不需要往返於伺服器要驗證的驗證控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-230">Unobtrusive validation allows the validation controls to use JavaScript for client-side validation logic, which means the page does not require a trip to the server to be validated.</span></span> <span data-ttu-id="638a7-231">根據預設，低調驗證會包含在*Web.config*檔案會根據下列的組態設定：</span><span class="sxs-lookup"><span data-stu-id="638a7-231">By default, unobtrusive validation is included in the *Web.config* file based on the following configuration setting:</span></span>

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a><span data-ttu-id="638a7-232">規則運算式</span><span class="sxs-lookup"><span data-stu-id="638a7-232">Regular Expressions</span></span>

<span data-ttu-id="638a7-233">產品價格*AdminPage.aspx*使用驗證頁面**RegularExpressionValidator**控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-233">The product price on the *AdminPage.aspx* page is validated using a **RegularExpressionValidator** control.</span></span> <span data-ttu-id="638a7-234">這個控制項驗證是否相關聯的輸入控制項 （"AddProductPrice 」 文字方塊） 的值符合規則運算式所指定的模式。</span><span class="sxs-lookup"><span data-stu-id="638a7-234">This control validates whether the value of the associated input control (the "AddProductPrice" TextBox) matches the pattern specified by the regular expression.</span></span> <span data-ttu-id="638a7-235">規則運算式是可讓您快速找出並符合特定的字元模式的模式比對標記法。</span><span class="sxs-lookup"><span data-stu-id="638a7-235">A regular expression is a pattern-matching notation that enables you to quickly find and match specific character patterns.</span></span> <span data-ttu-id="638a7-236">**RegularExpressionValidator**控制項包含一個名為屬性`ValidationExpression`，其中包含用來驗證價格輸入，如下所示的規則運算式：</span><span class="sxs-lookup"><span data-stu-id="638a7-236">The **RegularExpressionValidator** control includes a property named `ValidationExpression` that contains the regular expression used to validate price input, as shown below:</span></span>

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a><span data-ttu-id="638a7-237">FileUpload 控制項</span><span class="sxs-lookup"><span data-stu-id="638a7-237">FileUpload Control</span></span>

<span data-ttu-id="638a7-238">在您加入的輸入與驗證控制項，除了**FileUpload**若要控制*AdminPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-238">In addition to the input and validation controls, you added the **FileUpload** control to the *AdminPage.aspx* page.</span></span> <span data-ttu-id="638a7-239">這個控制項提供的功能，將檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="638a7-239">This control provides the capability to upload files.</span></span> <span data-ttu-id="638a7-240">在此情況下，您只允許上傳的映像檔。</span><span class="sxs-lookup"><span data-stu-id="638a7-240">In this case, you are only allowing image files to be uploaded.</span></span> <span data-ttu-id="638a7-241">在程式碼後置檔案 (*AdminPage.aspx.cs*)，當`AddProductButton`按一下時，程式碼會檢查`HasFile`屬性**FileUpload**控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-241">In the code-behind file (*AdminPage.aspx.cs*), when the `AddProductButton` is clicked, the code checks the `HasFile` property of the **FileUpload** control.</span></span> <span data-ttu-id="638a7-242">如果控制項有一個檔案，而且如果允許檔案類型 （根據副檔名而定），則影像會儲存到*映像*資料夾並*映像/個大拇指朝*應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="638a7-242">If the control has a file and if the file type (based on file extension) is allowed, the image is saved to the *Images* folder and the *Images/Thumbs* folder of the application.</span></span>

#### <a name="model-binding"></a><span data-ttu-id="638a7-243">模型繫結</span><span class="sxs-lookup"><span data-stu-id="638a7-243">Model Binding</span></span>

<span data-ttu-id="638a7-244">稍早在本教學課程系列中您用來填入模型繫結**ListView**控制**FormsView**控制**GridView**控制項，和**DetailView**控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-244">Earlier in this tutorial series you used model binding to populate a **ListView** control, a **FormsView** control, a **GridView** control, and a **DetailView** control.</span></span> <span data-ttu-id="638a7-245">在本教學課程中，您可以使用模型繫結填入**DropDownList**具有產品類別目錄的清單控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-245">In this tutorial, you use model binding to populate a **DropDownList** control with a list of product categories.</span></span>

<span data-ttu-id="638a7-246">您新增至標記*AdminPage.aspx*檔案包含**DropDownList**控制項稱為`DropDownAddCategory`:</span><span class="sxs-lookup"><span data-stu-id="638a7-246">The markup that you added to the *AdminPage.aspx* file contains a **DropDownList** control called `DropDownAddCategory`:</span></span>

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

<span data-ttu-id="638a7-247">您可以使用模型繫結來填入這**DropDownList**藉由設定`ItemType`屬性和`SelectMethod`屬性。</span><span class="sxs-lookup"><span data-stu-id="638a7-247">You use model binding to populate this **DropDownList** by setting the `ItemType` attribute and the `SelectMethod` attribute.</span></span> <span data-ttu-id="638a7-248">`ItemType`屬性會指定您使用`WingtipToys.Models.Category`輸入填入控制項時。</span><span class="sxs-lookup"><span data-stu-id="638a7-248">The `ItemType` attribute specifies that you use the `WingtipToys.Models.Category` type when populating the control.</span></span> <span data-ttu-id="638a7-249">您藉由建立定義此類型在開始本教學課程系列`Category`類別 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="638a7-249">You defined this type at the beginning of this tutorial series by creating the `Category` class (shown below).</span></span> <span data-ttu-id="638a7-250">`Category`類別位於*模型*資料夾內*Category.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="638a7-250">The `Category` class is in the *Models* folder inside the *Category.cs* file.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

<span data-ttu-id="638a7-251">`SelectMethod`的屬性**DropDownList**控制項可讓您指定您使用`GetCategories`方法 （如下所示） 也就是包含在程式碼後置檔案 (*AdminPage.aspx.cs*)。</span><span class="sxs-lookup"><span data-stu-id="638a7-251">The `SelectMethod` attribute of the **DropDownList** control specifies that you use the `GetCategories` method (shown below) that is included in the code-behind file (*AdminPage.aspx.cs*).</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

<span data-ttu-id="638a7-252">這個方法會指定`IQueryable`介面用來評估查詢，以針對`Category`型別。</span><span class="sxs-lookup"><span data-stu-id="638a7-252">This method specifies that an `IQueryable` interface is used to evaluate a query against a `Category` type.</span></span> <span data-ttu-id="638a7-253">傳回的值會用來填入**DropDownList**在網頁標記中 (*AdminPage.aspx*)。</span><span class="sxs-lookup"><span data-stu-id="638a7-253">The returned value is used to populate the **DropDownList** in the markup of the page (*AdminPage.aspx*).</span></span>

<span data-ttu-id="638a7-254">顯示在清單中的每個項目是藉由設定所指定的文字`DataTextField`屬性。</span><span class="sxs-lookup"><span data-stu-id="638a7-254">The text displayed for each item in the list is specified by setting the `DataTextField` attribute.</span></span> <span data-ttu-id="638a7-255">`DataTextField`屬性會使用`CategoryName`的`Category`（如上所示） 以顯示每個類別目錄中的類別**DropDownList**控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-255">The `DataTextField` attribute uses the `CategoryName` of the `Category` class (shown above) to display each category in the **DropDownList** control.</span></span> <span data-ttu-id="638a7-256">選取一個項目時所傳遞的實際值**DropDownList**控制根據`DataValueField`屬性。</span><span class="sxs-lookup"><span data-stu-id="638a7-256">The actual value that is passed when an item is selected in the **DropDownList** control is based on the `DataValueField` attribute.</span></span> <span data-ttu-id="638a7-257">`DataValueField`屬性設定為`CategoryID`中定義`Category`類別 （如上所示）。</span><span class="sxs-lookup"><span data-stu-id="638a7-257">The `DataValueField` attribute is set to the `CategoryID` as define in the `Category` class (shown above).</span></span>

### <a name="how-the-application-will-work"></a><span data-ttu-id="638a7-258">應用程式如何運作</span><span class="sxs-lookup"><span data-stu-id="638a7-258">How the Application Will Work</span></span>

<span data-ttu-id="638a7-259">當第一次，屬於"canEdit"角色的使用者瀏覽至網頁`DropDownAddCategory` **DropDownList**上面所述，控制會填入。</span><span class="sxs-lookup"><span data-stu-id="638a7-259">When the user belonging to the "canEdit" role navigates to the page for the first time, the `DropDownAddCategory`**DropDownList** control is populated as described above.</span></span> <span data-ttu-id="638a7-260">`DropDownRemoveProduct` **DropDownList**控制項也會填入產品使用相同的方法。</span><span class="sxs-lookup"><span data-stu-id="638a7-260">The `DropDownRemoveProduct`**DropDownList** control is also populated with products using the same approach.</span></span> <span data-ttu-id="638a7-261">選取類別目錄類型屬於"canEdit"角色的使用者，並加上產品詳細資料 (**名稱**，**描述**，**價格**，和**映像檔**).</span><span class="sxs-lookup"><span data-stu-id="638a7-261">The user belonging to the "canEdit" role selects the category type and adds product details (**Name**, **Description**, **Price**, and **Image File**).</span></span> <span data-ttu-id="638a7-262">當使用者屬於"canEdit"角色按一下**新增產品** 按鈕，`AddProductButton_Click`事件處理常式，就會觸發。</span><span class="sxs-lookup"><span data-stu-id="638a7-262">When the user belonging to the "canEdit" role clicks the **Add Product** button, the `AddProductButton_Click` event handler is triggered.</span></span> <span data-ttu-id="638a7-263">`AddProductButton_Click`事件處理常式位於程式碼後置檔案中 (*AdminPage.aspx.cs*) 會檢查以確定它符合允許的檔案類型的映像檔 *(.gif*， *.png*， *.jpeg*，或 *.jpg*)。</span><span class="sxs-lookup"><span data-stu-id="638a7-263">The `AddProductButton_Click` event handler located in the code-behind file (*AdminPage.aspx.cs*) checks the image file to make sure it matches the allowed file types *(.gif*, *.png*, *.jpeg*, or *.jpg*).</span></span> <span data-ttu-id="638a7-264">然後，映像檔會儲存至資料夾，Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="638a7-264">Then, the image file is saved into a folder of the Wingtip Toys sample application.</span></span> <span data-ttu-id="638a7-265">接下來，將新的產品加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="638a7-265">Next, the new product is added to the database.</span></span> <span data-ttu-id="638a7-266">若要完成新增新的產品的新執行個體`AddProducts`類別會建立並命名為產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-266">To accomplish adding a new product, a new instance of the `AddProducts` class is created and named products.</span></span> <span data-ttu-id="638a7-267">`AddProducts`類別具有名為方法`AddProduct`，產品物件呼叫這個方法，以將產品加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="638a7-267">The `AddProducts` class has a method named `AddProduct`, and the products object calls this method to add products to the database.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

<span data-ttu-id="638a7-268">如果程式碼已成功將新的產品加入資料庫中，頁面會重新查詢字串值載入`ProductAction=add`。</span><span class="sxs-lookup"><span data-stu-id="638a7-268">If the code successfully adds the new product to the database, the page is reloaded with the query string value `ProductAction=add`.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

<span data-ttu-id="638a7-269">當頁面重新載入時，查詢字串會包含在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="638a7-269">When the page reloads, the query string is included in the URL.</span></span> <span data-ttu-id="638a7-270">藉由重新載入頁面，屬於"canEdit"角色的使用者可以立即查看中的更新**DropDownList**上的控制項*AdminPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-270">By reloading the page, the user belonging to the "canEdit" role can immediately see the updates in the **DropDownList** controls on the *AdminPage.aspx* page.</span></span> <span data-ttu-id="638a7-271">此外，藉由包含查詢字串的 url，網頁可以顯示成功的訊息屬於"canEdit"角色的使用者。</span><span class="sxs-lookup"><span data-stu-id="638a7-271">Also, by including the query string with the URL, the page can display a success message to the user belonging to the "canEdit" role.</span></span>

<span data-ttu-id="638a7-272">當*AdminPage.aspx*頁面上 重新載入，`Page_Load`事件被呼叫。</span><span class="sxs-lookup"><span data-stu-id="638a7-272">When the *AdminPage.aspx* page reloads, the `Page_Load` event is called.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

<span data-ttu-id="638a7-273">`Page_Load`事件處理常式會檢查查詢字串值，並決定是否要顯示成功訊息。</span><span class="sxs-lookup"><span data-stu-id="638a7-273">The `Page_Load` event handler checks the query string value and determines whether to show a success message.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="638a7-274">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="638a7-274">Running the Application</span></span>

<span data-ttu-id="638a7-275">購物車 」 中，您可以執行的應用程式現在以查看您可以新增、 刪除和更新項目。</span><span class="sxs-lookup"><span data-stu-id="638a7-275">You can run the application now to see how you can add, delete, and update items in the shopping cart.</span></span> <span data-ttu-id="638a7-276">購物車總計將會反映在購物車中的所有項目的總成本。</span><span class="sxs-lookup"><span data-stu-id="638a7-276">The shopping cart total will reflect the total cost of all items in the shopping cart.</span></span>

1. <span data-ttu-id="638a7-277">在 [方案總管] 中，按下**F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="638a7-277">In Solution Explorer, press **F5** to run the Wingtip Toys sample application.</span></span>  
   <span data-ttu-id="638a7-278">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-278">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="638a7-279">按一下 **登入**在頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="638a7-279">Click the **Log in** link at the top of the page.</span></span> 

    ![成員資格和系統管理-登入連結](membership-and-administration/_static/image2.png)

   <span data-ttu-id="638a7-281">*Login.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="638a7-281">The *Login.aspx* page is displayed.</span></span>
3. <span data-ttu-id="638a7-282">使用下列的使用者名稱和密碼：</span><span class="sxs-lookup"><span data-stu-id="638a7-282">Use the following user name and password:</span></span>  
   <span data-ttu-id="638a7-283">使用者名稱： canEditUser@wingtiptoys.com</span><span class="sxs-lookup"><span data-stu-id="638a7-283">User name: canEditUser@wingtiptoys.com</span></span>  
   <span data-ttu-id="638a7-284">密碼： Pa $$ word1</span><span class="sxs-lookup"><span data-stu-id="638a7-284">Password: Pa$$word1</span></span> 

    ![成員資格和系統管理-登入頁面](membership-and-administration/_static/image3.png)
4. <span data-ttu-id="638a7-286">按一下 **登入**靠近頁面底部的按鈕。</span><span class="sxs-lookup"><span data-stu-id="638a7-286">Click the **Log in** button near the bottom of the page.</span></span>
5. <span data-ttu-id="638a7-287">在下一個頁面頂端，選取**系統管理員**連結以瀏覽至*AdminPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-287">At the top of the next page, select the **Admin** link to navigate to the *AdminPage.aspx* page.</span></span> 

    ![成員資格和系統管理-系統管理員連結](membership-and-administration/_static/image4.png)
6. <span data-ttu-id="638a7-289">若要測試輸入的驗證，請按一下**新增產品**而不加入任何產品詳細資料 按鈕。</span><span class="sxs-lookup"><span data-stu-id="638a7-289">To test the input validation, click the **Add Product** button without adding any product details.</span></span> 

    ![成員資格和系統管理-系統管理 頁面](membership-and-administration/_static/image5.png)

   <span data-ttu-id="638a7-291">請注意，必要的欄位會顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="638a7-291">Notice that the required field messages are displayed.</span></span>
7. <span data-ttu-id="638a7-292">新增新的產品的詳細資料，然後按一下**新增產品** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="638a7-292">Add the details for a new product, and then click the **Add Product** button.</span></span> 

    ![成員資格和系統管理-新增產品](membership-and-administration/_static/image6.png)
8. <span data-ttu-id="638a7-294">選取 **產品**加入從頂端導覽功能表，若要檢視新的產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-294">Select **Products** from the top navigation menu to view the new product you added.</span></span> 

    ![成員資格和系統管理-顯示新的產品](membership-and-administration/_static/image7.png)
9. <span data-ttu-id="638a7-296">按一下  **Admin**連結以返回 管理 頁面。</span><span class="sxs-lookup"><span data-stu-id="638a7-296">Click the **Admin** link to return to the administration page.</span></span>
10. <span data-ttu-id="638a7-297">在 **移除產品**一節的頁面上，選取您在加入新的產品**DropDownListBox**。</span><span class="sxs-lookup"><span data-stu-id="638a7-297">In the **Remove Product** section of the page, select the new product you added in the **DropDownListBox**.</span></span>
11. <span data-ttu-id="638a7-298">按一下 **移除產品**按鈕即可移除應用程式中的新產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-298">Click the **Remove Product** button to remove the new product from the application.</span></span> 

    ![成員資格和系統管理-移除產品](membership-and-administration/_static/image8.png)
12. <span data-ttu-id="638a7-300">選取 **產品**從頂端導覽功能表，確認已移除產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-300">Select **Products** from the top navigation menu to confirm that the product has been removed.</span></span>
13. <span data-ttu-id="638a7-301">按一下 **登出**存在於管理模式。</span><span class="sxs-lookup"><span data-stu-id="638a7-301">Click **Log off** to exist administration mode.</span></span>   
    <span data-ttu-id="638a7-302">請注意，不會再顯示頂端瀏覽窗格**Admin**功能表項目。</span><span class="sxs-lookup"><span data-stu-id="638a7-302">Notice that the top navigation pane no longer shows the **Admin** menu item.</span></span>

## <a name="summary"></a><span data-ttu-id="638a7-303">總結</span><span class="sxs-lookup"><span data-stu-id="638a7-303">Summary</span></span>

<span data-ttu-id="638a7-304">在本教學課程中，您可以加入自訂的角色和使用者屬於自訂角色，也就是受限制的存取權管理資料夾，以及頁面上，並提供自訂的角色為屬於使用者的瀏覽。</span><span class="sxs-lookup"><span data-stu-id="638a7-304">In this tutorial, you added a custom role and a user belonging to the custom role, restricted access to the administration folder and page, and provided navigation for the user belonging to the custom role.</span></span> <span data-ttu-id="638a7-305">您用來填入的模型繫結**DropDownList**控制項資料。</span><span class="sxs-lookup"><span data-stu-id="638a7-305">You used model binding to populate a **DropDownList** control with data.</span></span> <span data-ttu-id="638a7-306">您已實作**FileUpload**控制項和驗證控制項。</span><span class="sxs-lookup"><span data-stu-id="638a7-306">You implemented the **FileUpload** control and validation controls.</span></span> <span data-ttu-id="638a7-307">此外，您已了解如何新增和移除資料庫中的產品。</span><span class="sxs-lookup"><span data-stu-id="638a7-307">Also, you have learned how to add and remove products from a database.</span></span> <span data-ttu-id="638a7-308">在下一個教學課程中，您將了解如何實作 ASP.NET 路由。</span><span class="sxs-lookup"><span data-stu-id="638a7-308">In the next tutorial, you'll learn how to implement ASP.NET routing.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="638a7-309">其他資源</span><span class="sxs-lookup"><span data-stu-id="638a7-309">Additional Resources</span></span>

<span data-ttu-id="638a7-310">[Web.config 的 authorization 項目](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="638a7-310">[Web.config - authorization Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)</span></span>  
[<span data-ttu-id="638a7-311">ASP.NET 身分識別</span><span class="sxs-lookup"><span data-stu-id="638a7-311">ASP.NET Identity</span></span>](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[<span data-ttu-id="638a7-312">將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure 網站</span><span class="sxs-lookup"><span data-stu-id="638a7-312">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Web Site</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="638a7-313">Microsoft Azure-免費試用版</span><span class="sxs-lookup"><span data-stu-id="638a7-313">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> <span data-ttu-id="638a7-314">[上一頁](checkout-and-payment-with-paypal.md)
> [下一頁](url-routing.md)</span><span class="sxs-lookup"><span data-stu-id="638a7-314">[Previous](checkout-and-payment-with-paypal.md)
[Next](url-routing.md)</span></span>
