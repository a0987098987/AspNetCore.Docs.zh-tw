---
title: 在 ASP.NET 核心專案中向識別新增、下載和刪除使用者資料
author: rick-anderson
description: 瞭解如何在ASP.NET核心專案中向標識添加自定義用戶數據。 刪除每個 GDPR 的數據。
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 76b83df22381429feab80056c36dbdac1e5f20c7
ms.sourcegitcommit: 1d8f1396ccc66a0c3fcb5e5f36ea29b50db6d92a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501222"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="a9897-104">在 ASP.NET 核心項目中將自訂使用者資料新增、下載和刪除為識別</span><span class="sxs-lookup"><span data-stu-id="a9897-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="a9897-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9897-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9897-106">本文將說明如何：</span><span class="sxs-lookup"><span data-stu-id="a9897-106">This article shows how to:</span></span>

* <span data-ttu-id="a9897-107">將自定義用戶數據添加到ASP.NET核心 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="a9897-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="a9897-108">使用<xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>屬性標記自定義用戶數據模型,以便它自動可供下載和刪除。</span><span class="sxs-lookup"><span data-stu-id="a9897-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="a9897-109">使數據能夠下載和刪除有助於滿足[GDPR](xref:security/gdpr)要求。</span><span class="sxs-lookup"><span data-stu-id="a9897-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="a9897-110">專案示例是從 Razor Pages Web 應用創建的,但 ASP.NET 核心 MVC Web 應用的說明類似。</span><span class="sxs-lookup"><span data-stu-id="a9897-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a9897-111">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9897-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9897-112">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="a9897-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="a9897-113">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a9897-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a9897-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9897-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="a9897-115">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]\*\*\*\* > [專案]\*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="a9897-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="a9897-116">如果要命名專案**WebApp1,** 請將其與[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)代碼的命名空間匹配。</span><span class="sxs-lookup"><span data-stu-id="a9897-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="a9897-117">選擇**ASP.NET 核心 Web 應用程式**>**確定**</span><span class="sxs-lookup"><span data-stu-id="a9897-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="a9897-118">在下拉清單中**選擇ASP.NET核心 3.0**</span><span class="sxs-lookup"><span data-stu-id="a9897-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="a9897-119">選擇**Web 應用程式**>**確定**</span><span class="sxs-lookup"><span data-stu-id="a9897-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="a9897-120">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="a9897-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="a9897-121">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]\*\*\*\* > [專案]\*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="a9897-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="a9897-122">如果要命名專案**WebApp1,** 請將其與[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)代碼的命名空間匹配。</span><span class="sxs-lookup"><span data-stu-id="a9897-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="a9897-123">選擇**ASP.NET 核心 Web 應用程式**>**確定**</span><span class="sxs-lookup"><span data-stu-id="a9897-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="a9897-124">選擇 ASP.NET核心**2.2**</span><span class="sxs-lookup"><span data-stu-id="a9897-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="a9897-125">選擇**Web 應用程式**>**確定**</span><span class="sxs-lookup"><span data-stu-id="a9897-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="a9897-126">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="a9897-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="a9897-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9897-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="a9897-128">執行識別基架</span><span class="sxs-lookup"><span data-stu-id="a9897-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a9897-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9897-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9897-130">從**解決方案資源管理員**中,右鍵按一次專案>**新增新** > **的文手架項目**。</span><span class="sxs-lookup"><span data-stu-id="a9897-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a9897-131">從 **「添加基架」** 對話框的左邊窗格中,選擇 **「標識** > **添加**」 。</span><span class="sxs-lookup"><span data-stu-id="a9897-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="a9897-132">在 **'新增識別'** 對話框中, 以下選項:</span><span class="sxs-lookup"><span data-stu-id="a9897-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="a9897-133">選擇現有佈局檔 \**/頁面/共用/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a9897-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="a9897-134">選擇要覆寫的以下檔案:</span><span class="sxs-lookup"><span data-stu-id="a9897-134">Select the following files to override:</span></span>
    * <span data-ttu-id="a9897-135">**帳戶/註冊**</span><span class="sxs-lookup"><span data-stu-id="a9897-135">**Account/Register**</span></span>
    * <span data-ttu-id="a9897-136">**帳號/管理/索引**</span><span class="sxs-lookup"><span data-stu-id="a9897-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="a9897-137">選擇按鈕**+** 建立新**的資料內容 。**</span><span class="sxs-lookup"><span data-stu-id="a9897-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="a9897-138">接受類型 **(WebApp1.模型.WebApp1上下文**,如果專案名為**WebApp1**)。</span><span class="sxs-lookup"><span data-stu-id="a9897-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="a9897-139">選擇按鈕**+** 建立新**的使用者類別**。</span><span class="sxs-lookup"><span data-stu-id="a9897-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="a9897-140">接受類型(如果專案名為**WebApp1)>\*\*\*\*添加**, 則接受**WebApp1User。**</span><span class="sxs-lookup"><span data-stu-id="a9897-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="a9897-141">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="a9897-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a9897-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9897-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a9897-143">如果以前未安裝ASP.NET核心基架,請立即安裝:</span><span class="sxs-lookup"><span data-stu-id="a9897-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a9897-144">向[Microsoft.VisualStudio.Web.CodeGeneration.設計](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)添加對 Microsoft 的包引用(.csproj)檔。</span><span class="sxs-lookup"><span data-stu-id="a9897-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="a9897-145">在項目目錄中執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="a9897-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="a9897-146">執行以下指令以列出識別基架選項:</span><span class="sxs-lookup"><span data-stu-id="a9897-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="a9897-147">在項目資料夾中,執行識別基架:</span><span class="sxs-lookup"><span data-stu-id="a9897-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="a9897-148">依[移動、使用認證與佈局](xref:security/authentication/scaffold-identity#efm)中的說明執行以下步驟:</span><span class="sxs-lookup"><span data-stu-id="a9897-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="a9897-149">創建遷移並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="a9897-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="a9897-150">將 `UseAuthentication` 新增至 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="a9897-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="a9897-151">添加到`<partial name="_LoginPartial" />`佈局檔。</span><span class="sxs-lookup"><span data-stu-id="a9897-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="a9897-152">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="a9897-152">Test the app:</span></span>
  * <span data-ttu-id="a9897-153">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="a9897-153">Register a user</span></span>
  * <span data-ttu-id="a9897-154">選擇新的使用者名稱(**註銷**連結旁邊)。</span><span class="sxs-lookup"><span data-stu-id="a9897-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="a9897-155">您可能需要展開視窗或選擇導航列圖示以顯示使用者名和其他連結。</span><span class="sxs-lookup"><span data-stu-id="a9897-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="a9897-156">選擇 **『個人資料**』選項卡。</span><span class="sxs-lookup"><span data-stu-id="a9897-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="a9897-157">選擇 **「下載**」按鈕並檢查*個人資料.json*檔。</span><span class="sxs-lookup"><span data-stu-id="a9897-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="a9897-158">測試 **「刪除**」按鈕,該按鈕將刪除登錄的使用者。</span><span class="sxs-lookup"><span data-stu-id="a9897-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="a9897-159">將自訂使用者資料加入到識別資料庫</span><span class="sxs-lookup"><span data-stu-id="a9897-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="a9897-160">使用自定義`IdentityUser`屬性更新派生類。</span><span class="sxs-lookup"><span data-stu-id="a9897-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="a9897-161">如果命名專案 WebApp1,則檔名為 *"區域/標識/資料/WebApp1User.cs*"。</span><span class="sxs-lookup"><span data-stu-id="a9897-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="a9897-162">使用以下代碼更新檔案:</span><span class="sxs-lookup"><span data-stu-id="a9897-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="a9897-163">具有[「個人資料」](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)屬性的屬性包括:</span><span class="sxs-lookup"><span data-stu-id="a9897-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="a9897-164">刪除時*的區域/身份/頁面/帳戶/管理/刪除個人資料.cshtml*剃刀頁面呼`UserManager.Delete`叫 。</span><span class="sxs-lookup"><span data-stu-id="a9897-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="a9897-165">包含在下載的數據由*區域/身份/頁面/帳戶/管理/下載個人數據.cshtml*剃刀頁面。</span><span class="sxs-lookup"><span data-stu-id="a9897-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="a9897-166">更新帳號/管理/索引.cshtml 頁面</span><span class="sxs-lookup"><span data-stu-id="a9897-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="a9897-167">使用以下`InputModel`突顯的代碼更新*區域/識別/頁面/帳戶/管理/索引.cshtml.cs:*</span><span class="sxs-lookup"><span data-stu-id="a9897-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="a9897-168">使用以下突顯的標記更新*區域/識別/頁面/帳戶/管理/索引.cshtml:*</span><span class="sxs-lookup"><span data-stu-id="a9897-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="a9897-169">使用以下突顯的標記更新*區域/識別/頁面/帳戶/管理/索引.cshtml:*</span><span class="sxs-lookup"><span data-stu-id="a9897-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="a9897-170">更新帳號/註冊.cshtml 頁面</span><span class="sxs-lookup"><span data-stu-id="a9897-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="a9897-171">使用以下`InputModel`突顯的代碼更新*區域/識別/頁面/帳戶/註冊.cshtml.cs:*</span><span class="sxs-lookup"><span data-stu-id="a9897-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="a9897-172">使用以下突顯的標記更新*區域/標識/頁面/帳戶/註冊.cshtml:*</span><span class="sxs-lookup"><span data-stu-id="a9897-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="a9897-173">使用以下突顯的標記更新*區域/標識/頁面/帳戶/註冊.cshtml:*</span><span class="sxs-lookup"><span data-stu-id="a9897-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="a9897-174">建置專案。</span><span class="sxs-lookup"><span data-stu-id="a9897-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="a9897-175">為自訂使用者資料加入移轉</span><span class="sxs-lookup"><span data-stu-id="a9897-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a9897-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9897-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a9897-177">在視覺化工作室**套件管理員控制台中**:</span><span class="sxs-lookup"><span data-stu-id="a9897-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="a9897-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9897-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="a9897-179">測試建立、檢視、下載、刪除自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="a9897-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="a9897-180">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="a9897-180">Test the app:</span></span>

* <span data-ttu-id="a9897-181">註冊新使用者。</span><span class="sxs-lookup"><span data-stu-id="a9897-181">Register a new user.</span></span>
* <span data-ttu-id="a9897-182">查看頁面上的`/Identity/Account/Manage`自定義用戶數據。</span><span class="sxs-lookup"><span data-stu-id="a9897-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="a9897-183">從`/Identity/Account/Manage/PersonalData`頁面下載並查看使用者的個人數據。</span><span class="sxs-lookup"><span data-stu-id="a9897-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a><span data-ttu-id="a9897-184">使用 IUserClaims 主要工廠向識別新增聲明<ApplicationUser></span><span class="sxs-lookup"><span data-stu-id="a9897-184">Add claims to Identity using IUserClaimsPrincipalFactory<ApplicationUser></span></span>

<span data-ttu-id="a9897-185">可以使用`IUserClaimsPrincipalFactory<T>`介面將其他聲明添加到ASP.NET核心標識。</span><span class="sxs-lookup"><span data-stu-id="a9897-185">Additional claims can be added to ASP.NET Core Identity by using the `IUserClaimsPrincipalFactory<T>` interface.</span></span> <span data-ttu-id="a9897-186">類可以添加到`Startup.ConfigureServices`方法中的應用。</span><span class="sxs-lookup"><span data-stu-id="a9897-186">This class can be added to the app in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a9897-187">將類的自定義實現添加如下:</span><span class="sxs-lookup"><span data-stu-id="a9897-187">Add the custom implementation of the class as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

<span data-ttu-id="a9897-188">展示程式碼使用`ApplicationUser`類別 。</span><span class="sxs-lookup"><span data-stu-id="a9897-188">The demo code uses the `ApplicationUser` class.</span></span> <span data-ttu-id="a9897-189">此類添加用於`IsAdmin`添加附加聲明的屬性。</span><span class="sxs-lookup"><span data-stu-id="a9897-189">This class adds an `IsAdmin` property which is used to add the additional claim.</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

<span data-ttu-id="a9897-190">`AdditionalUserClaimsPrincipalFactory` 會實作 `UserClaimsPrincipalFactory` 介面。</span><span class="sxs-lookup"><span data-stu-id="a9897-190">The `AdditionalUserClaimsPrincipalFactory` implements the `UserClaimsPrincipalFactory` interface.</span></span> <span data-ttu-id="a9897-191">新角色宣告將新增到`ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="a9897-191">A new role claim is added to the `ClaimsPrincipal`.</span></span>

```csharp
public class AdditionalUserClaimsPrincipalFactory 
        : UserClaimsPrincipalFactory<ApplicationUser, IdentityRole>
{
    public AdditionalUserClaimsPrincipalFactory( 
        UserManager<ApplicationUser> userManager,
        RoleManager<IdentityRole> roleManager, 
        IOptions<IdentityOptions> optionsAccessor) 
        : base(userManager, roleManager, optionsAccessor)
    {}

    public async override Task<ClaimsPrincipal> CreateAsync(ApplicationUser user)
    {
        var principal = await base.CreateAsync(user);
        var identity = (ClaimsIdentity)principal.Identity;

        var claims = new List<Claim>();
        if (user.IsAdmin)
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "admin"));
        }
        else
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "user"));
        }

        identity.AddClaims(claims);
        return principal;
    }
}
```

<span data-ttu-id="a9897-192">然後,可以在應用中使用其他聲明。</span><span class="sxs-lookup"><span data-stu-id="a9897-192">The additional claim can then be used in the app.</span></span> <span data-ttu-id="a9897-193">在 Razor`IAuthorizationService`頁中 ,實例可用於訪問聲明值。</span><span class="sxs-lookup"><span data-stu-id="a9897-193">In a Razor Page, the `IAuthorizationService` instance can be used to access the claim value.</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@if ((await AuthorizationService.AuthorizeAsync(User, "IsAdmin")).Succeeded)
{
    <ul class="mr-auto navbar-nav">
        <li class="nav-item">
            <a class="nav-link" asp-controller="Admin" asp-action="Index">ADMIN</a>
        </li>
    </ul>
}
```
