---
title: 加入、 下載及刪除識別 ASP.NET Core 專案中的自訂使用者資料
author: rick-anderson
description: 了解如何自訂使用者資料加入 ASP.NET Core 專案中的身分識別。 刪除每個 GDPR 資料。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271953"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="4ffa7-104">加入、 下載及刪除識別 ASP.NET Core 專案中的自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="4ffa7-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="4ffa7-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ffa7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ffa7-106">本文將說明如何：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-106">This article shows how to:</span></span>

* <span data-ttu-id="4ffa7-107">加入 ASP.NET Core web 應用程式的自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="4ffa7-108">裝飾使用者自訂資料模型與[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性，讓自動可供下載和刪除。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="4ffa7-109">無法下載及刪除資料可協助符合[GDPR](xref:security/gdpr)需求。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="4ffa7-110">專案範例會建立從 Razor 頁面 web 應用程式，但的指示則類似 ASP.NET Core MVC web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4ffa7-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ffa7-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ffa7-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="4ffa7-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="4ffa7-113">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4ffa7-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ffa7-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ffa7-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4ffa7-115">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="4ffa7-116">將專案命名**WebApp1**如果您想要它符合命名空間[下載範例](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)程式碼。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="4ffa7-117">選取**ASP.NET Core Web 應用程式** > **[確定]**</span><span class="sxs-lookup"><span data-stu-id="4ffa7-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="4ffa7-118">選取**ASP.NET Core 2.1**下拉式清單中</span><span class="sxs-lookup"><span data-stu-id="4ffa7-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="4ffa7-119">選取**Web 應用程式**  > **[確定]**</span><span class="sxs-lookup"><span data-stu-id="4ffa7-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="4ffa7-120">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ffa7-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ffa7-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="4ffa7-122">執行身分識別 scaffolder</span><span class="sxs-lookup"><span data-stu-id="4ffa7-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ffa7-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ffa7-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4ffa7-124">從**方案總管 中**，以滑鼠右鍵按一下專案 >**新增** > **新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4ffa7-125">從左窗格的**新增 Scaffold**對話方塊中，選取**識別** > **新增**。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="4ffa7-126">在**新增身分識別**對話方塊中，下列選項：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="4ffa7-127">選取現有的版面配置檔案 *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4ffa7-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="4ffa7-128">選取要覆寫的下列檔案：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-128">Select the following files to override:</span></span>
    * <span data-ttu-id="4ffa7-129">**帳戶/註冊**</span><span class="sxs-lookup"><span data-stu-id="4ffa7-129">**Account/Register**</span></span>
    * <span data-ttu-id="4ffa7-130">**帳戶/管理/索引**</span><span class="sxs-lookup"><span data-stu-id="4ffa7-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="4ffa7-131">選取**+** 按鈕以建立新**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="4ffa7-132">接受的類型 (**WebApp1.Models.WebApp1Context**如果命名專案**WebApp1**)。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="4ffa7-133">選取**+** 按鈕以建立新**使用者類別**。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="4ffa7-134">接受的類型 (**WebApp1User**如果命名專案**WebApp1**) >**新增**。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="4ffa7-135">選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ffa7-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ffa7-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4ffa7-137">如果先前未安裝 ASP.NET scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="4ffa7-138">封裝將參考加入至[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)專案 (.csproj) 檔案。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="4ffa7-139">在專案目錄中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="4ffa7-140">執行下列命令來列出識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="4ffa7-141">在專案資料夾中，執行身分識別 scaffolder:</span><span class="sxs-lookup"><span data-stu-id="4ffa7-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="4ffa7-142">請依照下列中的指示[移轉、 UseAuthentication 和版面配置](xref:security/authentication/scaffold-identity#efm)執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="4ffa7-143">建立的移轉，並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="4ffa7-144">將 `UseAuthentication` 加入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="4ffa7-145">新增`<partial name="_LoginPartial" />`配置檔案。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="4ffa7-146">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-146">Test the app:</span></span>
  * <span data-ttu-id="4ffa7-147">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="4ffa7-147">Register a user</span></span>
  * <span data-ttu-id="4ffa7-148">選取新的使用者名稱 (旁**登出**連結)。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="4ffa7-149">您可能需要將視窗展開或選取瀏覽列圖示來顯示使用者名稱和其他連結。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="4ffa7-150">選取**個人資料** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="4ffa7-151">選取**下載**按鈕，然後檢查*PersonalData.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="4ffa7-152">測試**刪除**按鈕，這會刪除登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="4ffa7-153">識別資料庫中加入自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="4ffa7-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="4ffa7-154">更新`IdentityUser`衍生的類別具有自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="4ffa7-155">如果您專案 WebApp1，檔案名為*Areas/Identity/Data/WebApp1User.cs*。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="4ffa7-156">下列程式碼更新檔案：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="4ffa7-157">使用屬性來裝飾[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="4ffa7-158">刪除時*Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor 頁面呼叫`UserManager.Delete`。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="4ffa7-159">包含在所下載的資料*Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="4ffa7-160">更新 Account/Manage/Index.cshtml 頁面</span><span class="sxs-lookup"><span data-stu-id="4ffa7-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="4ffa7-161">更新`InputModel`中*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*以下列反白顯示程式碼：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="4ffa7-162">更新*Areas/Identity/Pages/Account/Manage/Index.cshtml*以反白顯示下列標記：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="4ffa7-163">更新 Account/Register.cshtml 頁面</span><span class="sxs-lookup"><span data-stu-id="4ffa7-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="4ffa7-164">更新`InputModel`中*Areas/Identity/Pages/Account/Register.cshtml.cs*以下列反白顯示程式碼：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="4ffa7-165">更新*Areas/Identity/Pages/Account/Register.cshtml*以反白顯示下列標記：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="4ffa7-166">建置專案。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="4ffa7-167">新增自訂使用者資料移轉</span><span class="sxs-lookup"><span data-stu-id="4ffa7-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ffa7-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ffa7-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4ffa7-169">在 Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="4ffa7-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ffa7-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ffa7-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="4ffa7-171">測試建立、 檢視、 下載、 刪除自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="4ffa7-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="4ffa7-172">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="4ffa7-172">Test the app:</span></span>

* <span data-ttu-id="4ffa7-173">註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-173">Register a new user.</span></span>
* <span data-ttu-id="4ffa7-174">檢視上的自訂使用者資料`/Identity/Account/Manage`頁面。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="4ffa7-175">下載並檢視中的使用者個人資料`/Identity/Account/Manage/PersonalData`頁面。</span><span class="sxs-lookup"><span data-stu-id="4ffa7-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
