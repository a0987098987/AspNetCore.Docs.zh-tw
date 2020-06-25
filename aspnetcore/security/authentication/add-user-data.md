---
title: 在 ASP.NET Core 專案中新增、下載及刪除使用者資料 Identity
author: rick-anderson
description: 瞭解如何 Identity 在 ASP.NET Core 專案中將自訂使用者資料新增至。 刪除每個 GDPR 的資料。
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/add-user-data
ms.openlocfilehash: 6b4de0a47cd7882852512040a08832942f20aa4c
ms.sourcegitcommit: 1833870ad0845326fb764fef1b530a07b9b5b099
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/25/2020
ms.locfileid: "85347108"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="abc3b-104">在 ASP.NET Core 專案中加入、下載和刪除自訂使用者資料 Identity</span><span class="sxs-lookup"><span data-stu-id="abc3b-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="abc3b-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="abc3b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="abc3b-106">本文將說明如何：</span><span class="sxs-lookup"><span data-stu-id="abc3b-106">This article shows how to:</span></span>

* <span data-ttu-id="abc3b-107">將自訂使用者資料新增至 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="abc3b-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="abc3b-108">將自訂使用者資料模型標記為 <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> 屬性，使其自動可供下載和刪除。</span><span class="sxs-lookup"><span data-stu-id="abc3b-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="abc3b-109">讓資料能夠下載和刪除有助於符合[GDPR](xref:security/gdpr)需求。</span><span class="sxs-lookup"><span data-stu-id="abc3b-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="abc3b-110">專案範例是從 Razor 頁面 web 應用程式建立的，但這些指示類似于 ASP.NET CORE MVC web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="abc3b-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="abc3b-111">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="abc3b-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abc3b-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="abc3b-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="abc3b-113">建立 Razor web 應用程式</span><span class="sxs-lookup"><span data-stu-id="abc3b-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="abc3b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="abc3b-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="abc3b-115">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]\*\*\*\* > [專案]\*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="abc3b-116">如果您想要符合[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="abc3b-117">選取 [ **ASP.NET Core Web 應用程式** > **正常]**</span><span class="sxs-lookup"><span data-stu-id="abc3b-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="abc3b-118">選取下拉式清單中的 [ **ASP.NET Core 3.0** ]</span><span class="sxs-lookup"><span data-stu-id="abc3b-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="abc3b-119">選取**Web 應用程式** > **正常**</span><span class="sxs-lookup"><span data-stu-id="abc3b-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="abc3b-120">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="abc3b-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="abc3b-121">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]\*\*\*\* > [專案]\*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="abc3b-122">如果您想要符合[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="abc3b-123">選取 [ **ASP.NET Core Web 應用程式** > **正常]**</span><span class="sxs-lookup"><span data-stu-id="abc3b-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="abc3b-124">選取下拉式清單中的 [ **ASP.NET Core 2.2** ]</span><span class="sxs-lookup"><span data-stu-id="abc3b-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="abc3b-125">選取**Web 應用程式** > **正常**</span><span class="sxs-lookup"><span data-stu-id="abc3b-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="abc3b-126">建置並執行專案。</span><span class="sxs-lookup"><span data-stu-id="abc3b-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="abc3b-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="abc3b-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="abc3b-128">執行 Identity scaffolder</span><span class="sxs-lookup"><span data-stu-id="abc3b-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="abc3b-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="abc3b-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="abc3b-130">在**方案總管**中，以滑鼠右鍵按一下專案 > [**加入**  >  **新的 scaffold 專案**]。</span><span class="sxs-lookup"><span data-stu-id="abc3b-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="abc3b-131">從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [ **Identity**  >  **新增**]。</span><span class="sxs-lookup"><span data-stu-id="abc3b-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="abc3b-132">在 [\*\*新增 Identity \*\* ] 對話方塊中，有下列選項：</span><span class="sxs-lookup"><span data-stu-id="abc3b-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="abc3b-133">選取現有的版面配置檔案 *~/Pages/Shared/_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="abc3b-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="abc3b-134">選取要覆寫的下列檔案：</span><span class="sxs-lookup"><span data-stu-id="abc3b-134">Select the following files to override:</span></span>
    * <span data-ttu-id="abc3b-135">**帳戶/註冊**</span><span class="sxs-lookup"><span data-stu-id="abc3b-135">**Account/Register**</span></span>
    * <span data-ttu-id="abc3b-136">**帳戶/管理/索引**</span><span class="sxs-lookup"><span data-stu-id="abc3b-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="abc3b-137">選取 [] **+** 按鈕以建立新的**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="abc3b-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="abc3b-138">如果專案名為**WebApp1**，請接受類型（**WebApp1. WebApp1CoNtext** ）。</span><span class="sxs-lookup"><span data-stu-id="abc3b-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="abc3b-139">選取 [] **+** 按鈕以建立新的**使用者類別**。</span><span class="sxs-lookup"><span data-stu-id="abc3b-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="abc3b-140">接受類型（如果專案名為**WebApp1**，則為**WebApp1User** ） >**新增**]。</span><span class="sxs-lookup"><span data-stu-id="abc3b-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="abc3b-141">選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="abc3b-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="abc3b-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="abc3b-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="abc3b-143">如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="abc3b-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="abc3b-144">將 Nswag.codegeneration.csharp 的套件參考新增至專案（.csproj）檔案。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) ）。</span><span class="sxs-lookup"><span data-stu-id="abc3b-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="abc3b-145">在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="abc3b-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="abc3b-146">執行下列命令以列出 Identity scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="abc3b-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="abc3b-147">在專案資料夾中，執行 Identity scaffolder：</span><span class="sxs-lookup"><span data-stu-id="abc3b-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="abc3b-148">依照 [遷移] [、[UseAuthentication] 和 [版面](xref:security/authentication/scaffold-identity#efm)配置] 中的指示執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abc3b-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="abc3b-149">建立遷移並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="abc3b-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="abc3b-150">將 `UseAuthentication` 新增至 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="abc3b-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="abc3b-151">將加入至配置檔案 `<partial name="_LoginPartial" />` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="abc3b-152">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="abc3b-152">Test the app:</span></span>
  * <span data-ttu-id="abc3b-153">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="abc3b-153">Register a user</span></span>
  * <span data-ttu-id="abc3b-154">選取新的使用者名稱（[**登出**] 連結旁）。</span><span class="sxs-lookup"><span data-stu-id="abc3b-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="abc3b-155">您可能需要展開視窗，或選取 [導覽列] 圖示，以顯示使用者名稱和其他連結。</span><span class="sxs-lookup"><span data-stu-id="abc3b-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="abc3b-156">選取 [**個人資料**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="abc3b-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="abc3b-157">選取 [**下載**] 按鈕，並*在檔案上檢查PersonalData.js* 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="abc3b-158">測試**刪除**按鈕，這會刪除已登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="abc3b-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="abc3b-159">將自訂使用者資料新增至 Identity 資料庫</span><span class="sxs-lookup"><span data-stu-id="abc3b-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="abc3b-160">`IdentityUser`以自訂屬性更新衍生類別。</span><span class="sxs-lookup"><span data-stu-id="abc3b-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="abc3b-161">如果您將專案命名為 WebApp1，檔案就會命名為*Areas/ Identity /Data/WebApp1User.cs*。</span><span class="sxs-lookup"><span data-stu-id="abc3b-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="abc3b-162">使用下列程式碼更新檔案：</span><span class="sxs-lookup"><span data-stu-id="abc3b-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="abc3b-163">具有[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)屬性的屬性如下：</span><span class="sxs-lookup"><span data-stu-id="abc3b-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="abc3b-164">當*區域/ Identity /Pages/Account/Manage/DeletePersonalData.cshtml* Razor 頁面呼叫時刪除 `UserManager.Delete` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="abc3b-165">包含在已下載資料中的 [*區域/ Identity /Pages/Account/Manage/DownloadPersonalData.cshtml* ] Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="abc3b-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="abc3b-166">更新 [帳戶/管理/索引. cshtml] 頁面</span><span class="sxs-lookup"><span data-stu-id="abc3b-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="abc3b-167">`InputModel`使用下列反白顯示的程式碼，更新*區域/ Identity /Pages/Account/Manage/Index.cshtml.cs*中的：</span><span class="sxs-lookup"><span data-stu-id="abc3b-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="abc3b-168">使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Manage/Index.cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="abc3b-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="abc3b-169">使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Manage/Index.cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="abc3b-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="abc3b-170">更新 [帳戶/註冊. cshtml] 頁面</span><span class="sxs-lookup"><span data-stu-id="abc3b-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="abc3b-171">`InputModel`使用下列反白顯示的程式碼，更新*區域/ Identity /Pages/Account/Register.cshtml.cs*中的：</span><span class="sxs-lookup"><span data-stu-id="abc3b-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="abc3b-172">使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Register.cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="abc3b-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="abc3b-173">使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Register.cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="abc3b-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="abc3b-174">建置專案。</span><span class="sxs-lookup"><span data-stu-id="abc3b-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="abc3b-175">新增自訂使用者資料的遷移</span><span class="sxs-lookup"><span data-stu-id="abc3b-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="abc3b-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="abc3b-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="abc3b-177">在 [Visual Studio**套件管理員主控台**] 中：</span><span class="sxs-lookup"><span data-stu-id="abc3b-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="abc3b-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="abc3b-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="abc3b-179">測試建立、查看、下載、刪除自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="abc3b-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="abc3b-180">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="abc3b-180">Test the app:</span></span>

* <span data-ttu-id="abc3b-181">註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="abc3b-181">Register a new user.</span></span>
* <span data-ttu-id="abc3b-182">在頁面上，查看自訂使用者資料 `/Identity/Account/Manage` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="abc3b-183">從頁面下載並查看使用者的個人資料 `/Identity/Account/Manage/PersonalData` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a><span data-ttu-id="abc3b-184">使用 IUserClaimsPrincipalFactory 將宣告新增至 Identity<ApplicationUser></span><span class="sxs-lookup"><span data-stu-id="abc3b-184">Add claims to Identity using IUserClaimsPrincipalFactory<ApplicationUser></span></span>

> [!NOTE]
> <span data-ttu-id="abc3b-185">本章節不是上一個教學課程的延伸。</span><span class="sxs-lookup"><span data-stu-id="abc3b-185">This section isn't an extension of the previous tutorial.</span></span> <span data-ttu-id="abc3b-186">若要將下列步驟套用至使用教學課程所建立的應用程式，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/18797)。</span><span class="sxs-lookup"><span data-stu-id="abc3b-186">To apply the following steps to the app built using the tutorial, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/18797).</span></span>

<span data-ttu-id="abc3b-187">您可以使用介面，將額外的宣告新增至 ASP.NET Core Identity `IUserClaimsPrincipalFactory<T>` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-187">Additional claims can be added to ASP.NET Core Identity by using the `IUserClaimsPrincipalFactory<T>` interface.</span></span> <span data-ttu-id="abc3b-188">這個類別可以新增至方法中的應用程式 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-188">This class can be added to the app in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="abc3b-189">新增類別的自訂執行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abc3b-189">Add the custom implementation of the class as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

<span data-ttu-id="abc3b-190">示範程式碼會使用 `ApplicationUser` 類別。</span><span class="sxs-lookup"><span data-stu-id="abc3b-190">The demo code uses the `ApplicationUser` class.</span></span> <span data-ttu-id="abc3b-191">這個類別會加入 `IsAdmin` 用來新增額外宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="abc3b-191">This class adds an `IsAdmin` property which is used to add the additional claim.</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

<span data-ttu-id="abc3b-192">`AdditionalUserClaimsPrincipalFactory` 會實作 `UserClaimsPrincipalFactory` 介面。</span><span class="sxs-lookup"><span data-stu-id="abc3b-192">The `AdditionalUserClaimsPrincipalFactory` implements the `UserClaimsPrincipalFactory` interface.</span></span> <span data-ttu-id="abc3b-193">新的角色宣告會加入至 `ClaimsPrincipal` 。</span><span class="sxs-lookup"><span data-stu-id="abc3b-193">A new role claim is added to the `ClaimsPrincipal`.</span></span>

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

<span data-ttu-id="abc3b-194">接著，您可以在應用程式中使用額外的宣告。</span><span class="sxs-lookup"><span data-stu-id="abc3b-194">The additional claim can then be used in the app.</span></span> <span data-ttu-id="abc3b-195">在 Razor 頁面中， `IAuthorizationService` 可以使用實例來存取宣告值。</span><span class="sxs-lookup"><span data-stu-id="abc3b-195">In a Razor Page, the `IAuthorizationService` instance can be used to access the claim value.</span></span>

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
