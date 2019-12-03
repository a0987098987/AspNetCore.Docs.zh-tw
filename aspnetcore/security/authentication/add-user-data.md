---
title: 在 ASP.NET Core 專案中新增、下載及刪除使用者資料至身分識別
author: rick-anderson
description: 瞭解如何將自訂使用者資料新增至 ASP.NET Core 專案中的身分識別。 刪除每個 GDPR 的資料。
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 6daca5776930f80eec8d81132b5a5c4d4d5c13ad
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681158"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="98522-104">在 ASP.NET Core 專案中新增、下載和刪除自訂使用者資料至身分識別</span><span class="sxs-lookup"><span data-stu-id="98522-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="98522-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="98522-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="98522-106">本文說明如何：</span><span class="sxs-lookup"><span data-stu-id="98522-106">This article shows how to:</span></span>

* <span data-ttu-id="98522-107">將自訂使用者資料新增至 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="98522-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="98522-108">使用 <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> 屬性裝飾自訂使用者資料模型，使其自動可供下載和刪除。</span><span class="sxs-lookup"><span data-stu-id="98522-108">Decorate the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="98522-109">讓資料能夠下載和刪除有助於符合[GDPR](xref:security/gdpr)需求。</span><span class="sxs-lookup"><span data-stu-id="98522-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="98522-110">專案範例是從 Razor Pages web 應用程式建立的，但這些指示類似于 ASP.NET Core MVC web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="98522-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="98522-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98522-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98522-112">必要條件：</span><span class="sxs-lookup"><span data-stu-id="98522-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="98522-113">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="98522-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98522-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98522-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="98522-115">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="98522-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="98522-116">如果您想要符合[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。</span><span class="sxs-lookup"><span data-stu-id="98522-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="98522-117">選取 [ **ASP.NET Core Web 應用程式**>**確定]**</span><span class="sxs-lookup"><span data-stu-id="98522-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="98522-118">選取下拉式清單中的 [ **ASP.NET Core 3.0** ]</span><span class="sxs-lookup"><span data-stu-id="98522-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="98522-119">選取 [ **Web 應用程式** **] > [確定]**</span><span class="sxs-lookup"><span data-stu-id="98522-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="98522-120">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="98522-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="98522-121">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="98522-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="98522-122">如果您想要符合[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。</span><span class="sxs-lookup"><span data-stu-id="98522-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="98522-123">選取 [ **ASP.NET Core Web 應用程式**>**確定]**</span><span class="sxs-lookup"><span data-stu-id="98522-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="98522-124">選取下拉式清單中的 [ **ASP.NET Core 2.2** ]</span><span class="sxs-lookup"><span data-stu-id="98522-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="98522-125">選取 [ **Web 應用程式** **] > [確定]**</span><span class="sxs-lookup"><span data-stu-id="98522-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="98522-126">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="98522-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="98522-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="98522-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="98522-128">執行身分識別 scaffolder</span><span class="sxs-lookup"><span data-stu-id="98522-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98522-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98522-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98522-130">在**方案總管**中，以滑鼠右鍵按一下專案 >**加入** > **新的 scaffold 專案**。</span><span class="sxs-lookup"><span data-stu-id="98522-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="98522-131">從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別** > **新增**]。</span><span class="sxs-lookup"><span data-stu-id="98522-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="98522-132">在 [**新增識別**] 對話方塊中，有下列選項：</span><span class="sxs-lookup"><span data-stu-id="98522-132">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="98522-133">選取現有的版面配置檔案 *~/Pages/Shared/_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="98522-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="98522-134">選取要覆寫的下列檔案：</span><span class="sxs-lookup"><span data-stu-id="98522-134">Select the following files to override:</span></span>
    * <span data-ttu-id="98522-135">**帳戶/註冊**</span><span class="sxs-lookup"><span data-stu-id="98522-135">**Account/Register**</span></span>
    * <span data-ttu-id="98522-136">**帳戶/管理/索引**</span><span class="sxs-lookup"><span data-stu-id="98522-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="98522-137">選取 [ **+** ] 按鈕，以建立新的**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="98522-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="98522-138">如果專案名為**WebApp1**，請接受類型（**WebApp1. WebApp1CoNtext** ）。</span><span class="sxs-lookup"><span data-stu-id="98522-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="98522-139">選取 [ **+** ] 按鈕，以建立新的**使用者類別**。</span><span class="sxs-lookup"><span data-stu-id="98522-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="98522-140">接受類型（如果專案名為**WebApp1**，則為**WebApp1User** ） >**新增**。</span><span class="sxs-lookup"><span data-stu-id="98522-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="98522-141">選取 [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="98522-141">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="98522-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="98522-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="98522-143">如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="98522-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="98522-144">將 Nswag.codegeneration.csharp 的套件參考新增至專案（.csproj）檔案。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) ）。</span><span class="sxs-lookup"><span data-stu-id="98522-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="98522-145">在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="98522-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="98522-146">執行下列命令以列出身分識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="98522-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="98522-147">在專案資料夾中，執行 身分識別 scaffolder：</span><span class="sxs-lookup"><span data-stu-id="98522-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="98522-148">依照 [遷移] [、[UseAuthentication] 和 [版面](xref:security/authentication/scaffold-identity#efm)配置] 中的指示執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="98522-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="98522-149">建立遷移並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="98522-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="98522-150">將 `UseAuthentication` 加入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="98522-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="98522-151">將 `<partial name="_LoginPartial" />` 新增至版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="98522-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="98522-152">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="98522-152">Test the app:</span></span>
  * <span data-ttu-id="98522-153">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="98522-153">Register a user</span></span>
  * <span data-ttu-id="98522-154">選取新的使用者名稱（[**登出**] 連結旁）。</span><span class="sxs-lookup"><span data-stu-id="98522-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="98522-155">您可能需要展開視窗，或選取 [導覽列] 圖示，以顯示使用者名稱和其他連結。</span><span class="sxs-lookup"><span data-stu-id="98522-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="98522-156">選取 [**個人資料**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="98522-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="98522-157">選取 [**下載**] 按鈕，並檢查*PersonalData*檔案。</span><span class="sxs-lookup"><span data-stu-id="98522-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="98522-158">測試**刪除**按鈕，這會刪除已登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="98522-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="98522-159">將自訂使用者資料新增至身分識別 DB</span><span class="sxs-lookup"><span data-stu-id="98522-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="98522-160">以自訂屬性更新 `IdentityUser` 的衍生類別。</span><span class="sxs-lookup"><span data-stu-id="98522-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="98522-161">如果您將專案命名為 WebApp1，檔案就會命名為*Areas/Identity/Data/WebApp1User .cs*。</span><span class="sxs-lookup"><span data-stu-id="98522-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="98522-162">使用下列程式碼更新檔案：</span><span class="sxs-lookup"><span data-stu-id="98522-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="98522-163">以[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)屬性裝飾的屬性包括：</span><span class="sxs-lookup"><span data-stu-id="98522-163">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="98522-164">當 [*區域/身分識別/頁面/帳戶/管理/DeletePersonalData* ] Razor 頁面呼叫 `UserManager.Delete`時刪除。</span><span class="sxs-lookup"><span data-stu-id="98522-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="98522-165">包含在下載的資料中，依*區域/身分識別/頁面/帳戶/管理/DownloadPersonalData. cshtml* Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="98522-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="98522-166">更新 [帳戶/管理/索引. cshtml] 頁面</span><span class="sxs-lookup"><span data-stu-id="98522-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="98522-167">使用下列反白顯示的程式碼，更新 [*區域/身分識別/頁面/帳戶/管理/索引*] 中的 `InputModel`：</span><span class="sxs-lookup"><span data-stu-id="98522-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="98522-168">以下列反白顯示的標記，更新*區域/身分識別/頁面/帳戶/管理/索引. cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="98522-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="98522-169">以下列反白顯示的標記，更新*區域/身分識別/頁面/帳戶/管理/索引. cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="98522-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="98522-170">更新 [帳戶/註冊. cshtml] 頁面</span><span class="sxs-lookup"><span data-stu-id="98522-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="98522-171">使用下列反白顯示的程式碼，更新*區域/身分識別/頁面/帳戶/登錄*中的 `InputModel`：</span><span class="sxs-lookup"><span data-stu-id="98522-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="98522-172">以下列醒目提示的標記更新 [*區域/身分識別/頁面/帳戶/* 暫存器]：</span><span class="sxs-lookup"><span data-stu-id="98522-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="98522-173">以下列醒目提示的標記更新 [*區域/身分識別/頁面/帳戶/* 暫存器]：</span><span class="sxs-lookup"><span data-stu-id="98522-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="98522-174">建置專案。</span><span class="sxs-lookup"><span data-stu-id="98522-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="98522-175">新增自訂使用者資料的遷移</span><span class="sxs-lookup"><span data-stu-id="98522-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98522-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98522-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="98522-177">在 [Visual Studio**套件管理員主控台**] 中：</span><span class="sxs-lookup"><span data-stu-id="98522-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="98522-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="98522-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="98522-179">測試建立、查看、下載、刪除自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="98522-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="98522-180">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="98522-180">Test the app:</span></span>

* <span data-ttu-id="98522-181">註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="98522-181">Register a new user.</span></span>
* <span data-ttu-id="98522-182">在 [`/Identity/Account/Manage`] 頁面上，查看自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="98522-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="98522-183">從 [`/Identity/Account/Manage/PersonalData`] 頁面下載並查看使用者個人資料。</span><span class="sxs-lookup"><span data-stu-id="98522-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
