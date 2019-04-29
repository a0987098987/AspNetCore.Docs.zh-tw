---
title: 加入、 下載及刪除身分識別的 ASP.NET Core 專案中的使用者資料
author: rick-anderson
description: 了解如何在 ASP.NET Core 專案中加入身分識別的自訂使用者資料。 刪除每 GDPR 的資料。
ms.author: riande
ms.date: 6/16/2018
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 1b7cac9206776e9290326511247b6db33f13cb32
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165085"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="e2937-104">加入、 下載及刪除身分識別的 ASP.NET Core 專案的自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="e2937-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="e2937-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e2937-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e2937-106">本文說明如何：</span><span class="sxs-lookup"><span data-stu-id="e2937-106">This article shows how to:</span></span>

* <span data-ttu-id="e2937-107">新增至 ASP.NET Core web 應用程式的自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="e2937-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="e2937-108">裝飾具有自訂使用者資料模型[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性，讓它自動可供下載和刪除。</span><span class="sxs-lookup"><span data-stu-id="e2937-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="e2937-109">無法下載及刪除資料可協助符合[GDPR](xref:security/gdpr)需求。</span><span class="sxs-lookup"><span data-stu-id="e2937-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="e2937-110">Razor 頁面 web 應用程式，從建立專案範例，但 ASP.NET Core MVC web 應用程式的指示如下。</span><span class="sxs-lookup"><span data-stu-id="e2937-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="e2937-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2937-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2937-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="e2937-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e2937-113">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="e2937-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2937-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2937-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2937-115">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="e2937-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="e2937-116">將專案命名為**WebApp1**如果您要符合的命名空間[下載範例](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)程式碼。</span><span class="sxs-lookup"><span data-stu-id="e2937-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="e2937-117">選取  **ASP.NET Core Web 應用程式** > **確定**</span><span class="sxs-lookup"><span data-stu-id="e2937-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="e2937-118">選取  **ASP.NET Core 2.2**下拉式清單中</span><span class="sxs-lookup"><span data-stu-id="e2937-118">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="e2937-119">選取  **Web 應用程式**  > **確定**</span><span class="sxs-lookup"><span data-stu-id="e2937-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="e2937-120">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="e2937-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2937-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e2937-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="e2937-122">執行身分識別框架</span><span class="sxs-lookup"><span data-stu-id="e2937-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2937-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2937-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2937-124">從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="e2937-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e2937-125">從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。</span><span class="sxs-lookup"><span data-stu-id="e2937-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="e2937-126">在  **ADD 身分識別**對話方塊中，下列選項：</span><span class="sxs-lookup"><span data-stu-id="e2937-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="e2937-127">選取現有的版面配置檔 *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e2937-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="e2937-128">選取要覆寫下列檔案：</span><span class="sxs-lookup"><span data-stu-id="e2937-128">Select the following files to override:</span></span>
    * <span data-ttu-id="e2937-129">**帳戶/註冊**</span><span class="sxs-lookup"><span data-stu-id="e2937-129">**Account/Register**</span></span>
    * <span data-ttu-id="e2937-130">**帳戶/管理/索引**</span><span class="sxs-lookup"><span data-stu-id="e2937-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="e2937-131">選取  **+** 來建立新的按鈕**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="e2937-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="e2937-132">接受的型別 (**WebApp1.Models.WebApp1Context**如果專案名為**WebApp1**)。</span><span class="sxs-lookup"><span data-stu-id="e2937-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="e2937-133">選取  **+** 來建立新的按鈕**使用者類別**。</span><span class="sxs-lookup"><span data-stu-id="e2937-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="e2937-134">接受的型別 (**WebApp1User**如果專案名為**WebApp1**) >**新增**。</span><span class="sxs-lookup"><span data-stu-id="e2937-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="e2937-135">選取 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e2937-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2937-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e2937-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e2937-137">如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="e2937-137">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="e2937-138">新增的套件參考[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)專案 (.csproj) 檔案。</span><span class="sxs-lookup"><span data-stu-id="e2937-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="e2937-139">在專案目錄中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e2937-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="e2937-140">執行下列命令來列出識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="e2937-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="e2937-141">在 [專案] 資料夾中，執行身分識別框架：</span><span class="sxs-lookup"><span data-stu-id="e2937-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="e2937-142">請依照下列中的指示[移轉、 UseAuthentication 和版面配置](xref:security/authentication/scaffold-identity#efm)來執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e2937-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="e2937-143">建立移轉並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="e2937-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="e2937-144">將 `UseAuthentication` 加入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="e2937-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="e2937-145">新增`<partial name="_LoginPartial" />`和配置檔案。</span><span class="sxs-lookup"><span data-stu-id="e2937-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="e2937-146">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="e2937-146">Test the app:</span></span>
  * <span data-ttu-id="e2937-147">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="e2937-147">Register a user</span></span>
  * <span data-ttu-id="e2937-148">選取新的使用者名稱 (旁**登出**連結)。</span><span class="sxs-lookup"><span data-stu-id="e2937-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="e2937-149">您可能需要展開視窗，或選取 瀏覽列圖示以顯示使用者名稱和其他連結。</span><span class="sxs-lookup"><span data-stu-id="e2937-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="e2937-150">選取 [**個人資料**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e2937-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="e2937-151">選取 **下載**按鈕，然後檢查*PersonalData.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e2937-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="e2937-152">測試**刪除**按鈕，這會刪除登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="e2937-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="e2937-153">識別資料庫中加入自訂的使用者資料</span><span class="sxs-lookup"><span data-stu-id="e2937-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="e2937-154">更新`IdentityUser`衍生的類別具有自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="e2937-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="e2937-155">如果您名為 WebApp1 的專案，檔案會命名為*Areas/Identity/Data/WebApp1User.cs*。</span><span class="sxs-lookup"><span data-stu-id="e2937-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="e2937-156">使用下列程式碼中更新檔案：</span><span class="sxs-lookup"><span data-stu-id="e2937-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="e2937-157">以屬性裝飾[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性：</span><span class="sxs-lookup"><span data-stu-id="e2937-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="e2937-158">刪除*Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor 頁面呼叫`UserManager.Delete`。</span><span class="sxs-lookup"><span data-stu-id="e2937-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="e2937-159">包含在所下載的資料*Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="e2937-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="e2937-160">更新 Account/Manage/Index.cshtml 頁面</span><span class="sxs-lookup"><span data-stu-id="e2937-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="e2937-161">更新`InputModel`中*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*下列醒目標示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e2937-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="e2937-162">更新*Areas/Identity/Pages/Account/Manage/Index.cshtml*下列反白顯示的標記：</span><span class="sxs-lookup"><span data-stu-id="e2937-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="e2937-163">更新 Account/Register.cshtml 頁面</span><span class="sxs-lookup"><span data-stu-id="e2937-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="e2937-164">更新`InputModel`中*Areas/Identity/Pages/Account/Register.cshtml.cs*下列醒目標示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e2937-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="e2937-165">更新*Areas/Identity/Pages/Account/Register.cshtml*下列反白顯示的標記：</span><span class="sxs-lookup"><span data-stu-id="e2937-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="e2937-166">建置專案。</span><span class="sxs-lookup"><span data-stu-id="e2937-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="e2937-167">新增自訂使用者資料移轉</span><span class="sxs-lookup"><span data-stu-id="e2937-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2937-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2937-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e2937-169">在 Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="e2937-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2937-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e2937-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="e2937-171">測試建立、 檢視、 下載、 刪除自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="e2937-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="e2937-172">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="e2937-172">Test the app:</span></span>

* <span data-ttu-id="e2937-173">註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="e2937-173">Register a new user.</span></span>
* <span data-ttu-id="e2937-174">檢視上的自訂使用者資料`/Identity/Account/Manage`頁面。</span><span class="sxs-lookup"><span data-stu-id="e2937-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="e2937-175">下載並檢視中的使用者個人資料`/Identity/Account/Manage/PersonalData`頁面。</span><span class="sxs-lookup"><span data-stu-id="e2937-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
