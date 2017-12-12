---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: "建立 MVC 3 應用程式使用 Razor 和不顯眼的 JavaScript |Microsoft 文件"
author: microsoft
description: "使用者清單的範例 web 應用程式示範如何建立 ASP.NET MVC 3 應用程式使用 Razor 檢視引擎是簡單。 範例應用程式 s 中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 68870caf1608e596962650cf653e5b455b82382a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="12df6-104">建立 MVC 3 應用程式使用 Razor 和不顯眼的 JavaScript</span><span class="sxs-lookup"><span data-stu-id="12df6-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="12df6-105">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="12df6-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="12df6-106">使用者清單的範例 web 應用程式示範如何建立 ASP.NET MVC 3 應用程式使用 Razor 檢視引擎是簡單。</span><span class="sxs-lookup"><span data-stu-id="12df6-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="12df6-107">範例應用程式示範如何使用新的 Razor 檢視引擎，使用 ASP.NET MVC 3 版和 Visual Studio 2010 來建立虛構的使用者清單網站包含功能，例如建立、 顯示、 編輯和刪除使用者。</span><span class="sxs-lookup"><span data-stu-id="12df6-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="12df6-108">本教學課程描述才能建立使用者清單的範例 ASP.NET MVC 3 應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="12df6-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="12df6-109">本主題隨附了與 C# 和 VB 原始碼的 Visual Studio 專案：[下載](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114)。</span><span class="sxs-lookup"><span data-stu-id="12df6-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="12df6-110">如果您有本教學課程有關的問題，請張貼到[MVC 論壇](https://forums.asp.net/1146.aspx)。</span><span class="sxs-lookup"><span data-stu-id="12df6-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="12df6-111">概觀</span><span class="sxs-lookup"><span data-stu-id="12df6-111">Overview</span></span>

<span data-ttu-id="12df6-112">您將建置的應用程式是一個簡單的使用者清單的網站。</span><span class="sxs-lookup"><span data-stu-id="12df6-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="12df6-113">使用者可以輸入、 檢視和更新使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="12df6-113">Users can enter, view, and update user information.</span></span>

![範例站台](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="12df6-115">您可以下載 VB 和 C# 已完成的專案[這裡](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)。</span><span class="sxs-lookup"><span data-stu-id="12df6-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="12df6-116">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="12df6-116">Creating the Web Application</span></span>

<span data-ttu-id="12df6-117">若要開始本教學課程，請開啟 Visual Studio 2010 並建立新專案使用*ASP.NET MVC 3 Web 應用程式*範本。</span><span class="sxs-lookup"><span data-stu-id="12df6-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="12df6-118">將應用程式命名&quot;Mvc3Razor&quot;。</span><span class="sxs-lookup"><span data-stu-id="12df6-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="12df6-119">[![新的 MVC 3 專案](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="12df6-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="12df6-120">在**新增 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**選取 Razor 檢視引擎，，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="12df6-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![新的 ASP.NET MVC 3 專案對話方塊](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="12df6-122">本教學課程中您不使用 ASP.NET 成員資格提供者，因此您可以刪除與登入和成員資格相關聯的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="12df6-123">在**方案總管 中**，移除下列檔案和目錄：</span><span class="sxs-lookup"><span data-stu-id="12df6-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="12df6-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="12df6-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="12df6-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="12df6-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="12df6-126">*_Layout.cshtml\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="12df6-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="12df6-127">*Views\Account* （及此目錄中的所有檔案）</span><span class="sxs-lookup"><span data-stu-id="12df6-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="12df6-129">編輯 *\_Layout.cshtml*檔案，並將內部標記`<div>`名`logindisplay`訊息 *&quot;*登入已停用&quot;.</span><span class="sxs-lookup"><span data-stu-id="12df6-129">Edit the *\_Layout.cshtml* file and replace the markup inside the `<div>` element named `logindisplay` with the message *&quot;*Login Disabled&quot;.</span></span> <span data-ttu-id="12df6-130">下列範例會顯示新標記：</span><span class="sxs-lookup"><span data-stu-id="12df6-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="12df6-131">加入模型</span><span class="sxs-lookup"><span data-stu-id="12df6-131">Adding the Model</span></span>

<span data-ttu-id="12df6-132">在**方案總管] 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後按一下 [**類別**。</span><span class="sxs-lookup"><span data-stu-id="12df6-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![新使用者 Mdl 類別](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="12df6-134">將類別命名為 `UserModel` 。</span><span class="sxs-lookup"><span data-stu-id="12df6-134">Name the class `UserModel`.</span></span> <span data-ttu-id="12df6-135">取代內容*UserModel*以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="12df6-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="12df6-136">`UserModel`類別代表使用者。</span><span class="sxs-lookup"><span data-stu-id="12df6-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="12df6-137">類別的每個成員都以註解[需要](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx)屬性從[DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)命名空間。</span><span class="sxs-lookup"><span data-stu-id="12df6-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="12df6-138">中的屬性[DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)命名空間提供 web 應用程式的自動用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="12df6-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="12df6-139">開啟`HomeController`類別，然後將`using`指示詞，好讓您可以存取`UserModel`和`Users`類別：</span><span class="sxs-lookup"><span data-stu-id="12df6-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="12df6-140">後方`HomeController`宣告中，加入下列註解和參考`Users`類別：</span><span class="sxs-lookup"><span data-stu-id="12df6-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="12df6-141">`Users`類別是經過簡化的記憶體中的資料存放區，您將使用在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="12df6-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="12df6-142">在實際的應用程式中，您會使用資料庫來儲存使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="12df6-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="12df6-143">前的幾行`HomeController`檔案會在下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="12df6-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="12df6-144">建置應用程式，如此使用者模型將 scaffolding 精靈中下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="12df6-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="12df6-145">建立預設檢視</span><span class="sxs-lookup"><span data-stu-id="12df6-145">Creating the Default View</span></span>

<span data-ttu-id="12df6-146">下一個步驟是加入動作方法和檢視來顯示的使用者。</span><span class="sxs-lookup"><span data-stu-id="12df6-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="12df6-147">刪除現有*Views\Home\Index*檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="12df6-148">您將建立新*索引*来顯示的使用者檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="12df6-149">在`HomeController`類別中，取代內容`Index`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="12df6-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="12df6-150">以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="12df6-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![加入檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="12df6-152">選取**建立強型別檢視**選項。</span><span class="sxs-lookup"><span data-stu-id="12df6-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="12df6-153">如**檢視資料類別**，選取**Mvc3Razor.Models.UserModel**。</span><span class="sxs-lookup"><span data-stu-id="12df6-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="12df6-154">(如果您沒有看到**Mvc3Razor.Models.UserModel**中**檢視資料類別** 方塊中，您必須先建置專案。)請確定檢視引擎設**Razor**。</span><span class="sxs-lookup"><span data-stu-id="12df6-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="12df6-155">設定**檢視內容**至**清單**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="12df6-155">Set **View content** to **List** and then click **Add**.</span></span>

![加入索引檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="12df6-157">新的檢視自動 scaffolds 使用者資料傳遞至`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="12df6-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="12df6-158">檢查新產生*Views\Home\Index*檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="12df6-159">**新建**，**編輯**，**詳細資料**，和**刪除**連結無法運作，而其他頁面的功能。</span><span class="sxs-lookup"><span data-stu-id="12df6-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="12df6-160">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="12df6-160">Run the page.</span></span> <span data-ttu-id="12df6-161">您看到使用者的清單。</span><span class="sxs-lookup"><span data-stu-id="12df6-161">You see a list of users.</span></span>

![索引頁](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="12df6-163">開啟*Index.cshtml*檔案，並將`ActionLink`標記**編輯**，**詳細資料**，和**刪除**為下列程式碼:</span><span class="sxs-lookup"><span data-stu-id="12df6-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="12df6-164">使用者名稱會做為識別碼尋找中的選定的記錄**編輯**，**詳細資料**，和**刪除**連結。</span><span class="sxs-lookup"><span data-stu-id="12df6-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="12df6-165">建立詳細資料檢視</span><span class="sxs-lookup"><span data-stu-id="12df6-165">Creating the Details View</span></span>

<span data-ttu-id="12df6-166">下一個步驟是加入`Details`動作方法，以顯示使用者詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="12df6-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![詳細資料](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="12df6-168">加入下列`Details`主控制器的方法：</span><span class="sxs-lookup"><span data-stu-id="12df6-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="12df6-169">以滑鼠右鍵按一下`Details`方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="12df6-169">Right-click inside the `Details` method and then select **Add View**.</span></span> <span data-ttu-id="12df6-170">確認**檢視資料類別**方塊包含**Mvc3Razor.Models.UserModel***。*</span><span class="sxs-lookup"><span data-stu-id="12df6-170">Verify that the **View data class** box contains **Mvc3Razor.Models.UserModel***.*</span></span> <span data-ttu-id="12df6-171">設定**檢視內容**至**詳細資料**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="12df6-171">Set **View content** to **Details** and then click **Add**.</span></span>

![加入詳細資料檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="12df6-173">執行應用程式並選取詳細資料 連結。</span><span class="sxs-lookup"><span data-stu-id="12df6-173">Run the application and select a details link.</span></span> <span data-ttu-id="12df6-174">自動 scaffolding 顯示模型中的每一個屬性。</span><span class="sxs-lookup"><span data-stu-id="12df6-174">The automatic scaffolding shows each property in the model.</span></span>

![詳細資料](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="12df6-176">建立編輯檢視</span><span class="sxs-lookup"><span data-stu-id="12df6-176">Creating the Edit View</span></span>

<span data-ttu-id="12df6-177">加入下列`Edit`主控制器的方法。</span><span class="sxs-lookup"><span data-stu-id="12df6-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="12df6-178">加入檢視如同先前的步驟，但設定**檢視內容**至**編輯**。</span><span class="sxs-lookup"><span data-stu-id="12df6-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![加入編輯檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="12df6-180">執行應用程式，並編輯一位使用者的第一個和最後一個名稱。</span><span class="sxs-lookup"><span data-stu-id="12df6-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="12df6-181">如果違反任何`DataAnnotation`已套用至的條件約束`UserModel`類別，當您送出表單時，您會看到伺服端程式碼所產生的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="12df6-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="12df6-182">比方說，如果您變更名字&quot;王&quot;至&quot;A&quot;，當您提交表單，表單上顯示下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="12df6-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="12df6-183">在本教學課程中，您要將使用者名稱視為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="12df6-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="12df6-184">因此，無法變更使用者名稱屬性。</span><span class="sxs-lookup"><span data-stu-id="12df6-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="12df6-185">在*Edit.cshtml*檔案，就在`Html.BeginForm`陳述式中，設定使用者名稱是隱藏的欄位。</span><span class="sxs-lookup"><span data-stu-id="12df6-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="12df6-186">這會導致要傳入模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="12df6-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="12df6-187">下列程式碼片段顯示的位置`Hidden`陳述式：</span><span class="sxs-lookup"><span data-stu-id="12df6-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="12df6-188">取代`TextBoxFor`和`ValidationMessageFor`標記的使用者名稱與`DisplayFor`呼叫。</span><span class="sxs-lookup"><span data-stu-id="12df6-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="12df6-189">`DisplayFor`方法顯示的屬性為唯讀狀態的項目。</span><span class="sxs-lookup"><span data-stu-id="12df6-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="12df6-190">下列範例顯示完整的標記。</span><span class="sxs-lookup"><span data-stu-id="12df6-190">The following example shows the completed markup.</span></span> <span data-ttu-id="12df6-191">原始`TextBoxFor`和`ValidationMessageFor`呼叫標記為註解 Razor 開始註解，然後結束註解字元 (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="12df6-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="12df6-192">啟用用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="12df6-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="12df6-193">若要啟用 ASP.NET MVC 3 中的用戶端驗證，您必須設定兩個旗標，而且您必須包含三個的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="12df6-194">開啟應用程式的*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="12df6-195">確認`that ClientValidationEnabled`和`UnobtrusiveJavaScriptEnabled`設為應用程式設定中，則為 true。</span><span class="sxs-lookup"><span data-stu-id="12df6-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="12df6-196">下列片段從根*Web.config*檔案會顯示正確的設定：</span><span class="sxs-lookup"><span data-stu-id="12df6-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="12df6-197">設定`UnobtrusiveJavaScriptEnabled`來啟用不顯眼的 Ajax 和不顯眼的用戶端驗證，則為 true。</span><span class="sxs-lookup"><span data-stu-id="12df6-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="12df6-198">當您使用不顯眼的驗證時，驗證規則會變成 HTML5 屬性。</span><span class="sxs-lookup"><span data-stu-id="12df6-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="12df6-199">HTML5 屬性名稱可以包含小寫字母、 數字和連字號。</span><span class="sxs-lookup"><span data-stu-id="12df6-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="12df6-200">設定`ClientValidationEnabled`true 會啟用用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="12df6-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="12df6-201">藉由在應用程式中設定這些機碼*Web.config*檔案中，您可以啟用用戶端驗證和不顯眼的 JavaScript，整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="12df6-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="12df6-202">您也可以啟用或停用這些設定個別的檢視，或使用下列程式碼的控制器方法：</span><span class="sxs-lookup"><span data-stu-id="12df6-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="12df6-203">您也需要呈現的檢視中包含數個 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="12df6-204">若要在所有檢視中包含 JavaScript 的簡單方法是將其新增至*_layout.cshtml\\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="12df6-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="12df6-205">取代`<head>`元素 *\_Layout.cshtml*以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="12df6-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="12df6-206">前兩個 jQuery 指令碼會裝載由 Microsoft Ajax 內容傳遞網路 (CDN)。</span><span class="sxs-lookup"><span data-stu-id="12df6-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="12df6-207">利用 Microsoft Ajax CDN，您可以大幅改善應用程式的第一個叫用次數效能。</span><span class="sxs-lookup"><span data-stu-id="12df6-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="12df6-208">執行應用程式，然後按一下 [編輯] 連結。</span><span class="sxs-lookup"><span data-stu-id="12df6-208">Run the application and click an edit link.</span></span> <span data-ttu-id="12df6-209">在瀏覽器中檢視網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="12df6-209">View the page's source in the browser.</span></span> <span data-ttu-id="12df6-210">瀏覽器來源顯示表單的許多屬性`data-val`（適用於資料驗證）。</span><span class="sxs-lookup"><span data-stu-id="12df6-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="12df6-211">啟用用戶端驗證和不顯眼的 JavaScript 時，與用戶端驗證規則的輸入的欄位會包含`data-val="true"`觸發不顯眼的用戶端驗證的屬性。</span><span class="sxs-lookup"><span data-stu-id="12df6-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="12df6-212">比方說，`City`模型中的欄位以裝飾[需要](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx)屬性，這會產生 HTML，下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="12df6-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="12df6-213">針對每個用戶端驗證規則，將屬性加入具有表單`data-val-rulename="message"`。</span><span class="sxs-lookup"><span data-stu-id="12df6-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="12df6-214">使用`City`欄位先前範例所示，必要的用戶端驗證規則會產生`data-val-required`屬性和訊息&quot;縣 （市） 欄位是必要的&quot;。</span><span class="sxs-lookup"><span data-stu-id="12df6-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="12df6-215">執行應用程式，請編輯一位使用者，並清除`City`欄位。</span><span class="sxs-lookup"><span data-stu-id="12df6-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="12df6-216">當您從欄位索引標籤上時，您會看到用戶端驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="12df6-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![所需的縣 （市)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="12df6-218">同樣地，用戶端驗證規則中每一個參數，將屬性加入具有表單`data-val-rulename-paramname=paramvalue`。</span><span class="sxs-lookup"><span data-stu-id="12df6-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="12df6-219">例如，`FirstName`附註屬性[StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性，並且指定最小長度為 3 和最大長度為 8。</span><span class="sxs-lookup"><span data-stu-id="12df6-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="12df6-220">資料驗證規則，名為`length`具有參數名稱`max`和參數值 8。</span><span class="sxs-lookup"><span data-stu-id="12df6-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="12df6-221">下圖顯示針對所產生的 HTML`FirstName`欄位編輯一位使用者時：</span><span class="sxs-lookup"><span data-stu-id="12df6-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="12df6-222">如需不顯眼的用戶端驗證的詳細資訊，請參閱文章[ASP.NET MVC 3 中不顯眼的用戶端驗證](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)Brad Wilson 的部落格。</span><span class="sxs-lookup"><span data-stu-id="12df6-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="12df6-223">ASP.NET MVC 3 在 beta 版中，您有時需要送出表單時，才能啟動用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="12df6-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="12df6-224">這可能會變更的最終版本。</span><span class="sxs-lookup"><span data-stu-id="12df6-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="12df6-225">建立建立檢視</span><span class="sxs-lookup"><span data-stu-id="12df6-225">Creating the Create View</span></span>

<span data-ttu-id="12df6-226">下一個步驟是加入`Create`動作方法，若要讓使用者建立新的使用者檢視。</span><span class="sxs-lookup"><span data-stu-id="12df6-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="12df6-227">加入下列`Create`主控制器的方法：</span><span class="sxs-lookup"><span data-stu-id="12df6-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="12df6-228">加入檢視如同先前的步驟，但設定**檢視內容**至**建立**。</span><span class="sxs-lookup"><span data-stu-id="12df6-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![建立檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="12df6-230">執行應用程式中，選取**建立**連結，並加入新的使用者。</span><span class="sxs-lookup"><span data-stu-id="12df6-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="12df6-231">`Create`方法自動會利用用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="12df6-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="12df6-232">嘗試輸入使用者名稱包含空格，例如&quot;Ben X&quot;。</span><span class="sxs-lookup"><span data-stu-id="12df6-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="12df6-233">當您從 [使用者名稱] 欄位，用戶端驗證錯誤的索引標籤 (`White space is not allowed`) 隨即出現。</span><span class="sxs-lookup"><span data-stu-id="12df6-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="12df6-234">新增 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="12df6-234">Add the Delete method</span></span>

<span data-ttu-id="12df6-235">若要完成本教學課程，加入下列`Delete`主控制器的方法：</span><span class="sxs-lookup"><span data-stu-id="12df6-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="12df6-236">新增`Delete`如同先前的步驟，設定檢視**檢視內容**至**刪除**。</span><span class="sxs-lookup"><span data-stu-id="12df6-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![刪除檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="12df6-238">您現在有簡單但功能完整 ASP.NET MVC 3 應用程式驗證。</span><span class="sxs-lookup"><span data-stu-id="12df6-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
