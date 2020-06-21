---
title: 搭配 WebAssembly 使用 ASP.NET Core SignalR Blazor
author: guardrex
description: 建立使用 ASP.NET Core 搭配 WebAssembly 的聊天應用程式 SignalR Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: 3f8aeec1e0471bab5034d1dcc8a42023f6b13c0d
ms.sourcegitcommit: 77729ba225d5143c0e3954db005906f4a5c7da95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2020
ms.locfileid: "85122096"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="87ec6-103">搭配 WebAssembly 使用 ASP.NET Core SignalR Blazor</span><span class="sxs-lookup"><span data-stu-id="87ec6-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="87ec6-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87ec6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="87ec6-105">本教學課程將教您使用 with WebAssembly 建立即時應用程式的基本概念 SignalR Blazor 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="87ec6-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="87ec6-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="87ec6-107">建立 Blazor WebAssembly 託管應用程式專案</span><span class="sxs-lookup"><span data-stu-id="87ec6-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="87ec6-108">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="87ec6-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="87ec6-109">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="87ec6-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="87ec6-110">新增 SignalR 服務和中樞的端點 SignalR</span><span class="sxs-lookup"><span data-stu-id="87ec6-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="87ec6-111">新增 Razor 聊天的元件程式碼</span><span class="sxs-lookup"><span data-stu-id="87ec6-111">Add Razor component code for chat</span></span>

<span data-ttu-id="87ec6-112">在本教學課程結尾，您將會有一個可運作的聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="87ec6-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="87ec6-113">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="87ec6-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87ec6-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="87ec6-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="87ec6-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87ec6-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="87ec6-116">使用**ASP.NET 和 網頁程式開發**工作負載[Visual Studio 2019 16.6 或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="87ec6-116">[Visual Studio 2019 16.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="87ec6-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87ec6-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="87ec6-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="87ec6-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="87ec6-119">Visual Studio for Mac 8.6 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="87ec6-119">Visual Studio for Mac version 8.6 or later</span></span>](https://visualstudio.microsoft.com/vs/mac/)
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="87ec6-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="87ec6-120">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="87ec6-121">建立 hosted Blazor WebAssembly 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="87ec6-121">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="87ec6-122">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="87ec6-122">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="87ec6-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87ec6-123">Visual Studio</span></span>](#tab/visual-studio)

> [!NOTE]
> <span data-ttu-id="87ec6-124">Visual Studio 16.6 或更新版本，而且需要 .NET Core SDK 3.1.300 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="87ec6-124">Visual Studio 16.6 or later and .NET Core SDK 3.1.300 or later are required.</span></span>

1. <span data-ttu-id="87ec6-125">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="87ec6-125">Create a new project.</span></span>

1. <span data-ttu-id="87ec6-126">選取\*\* Blazor 應用程式\*\*，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="87ec6-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="87ec6-127">`BlazorSignalRApp`在 [**專案名稱**] 欄位中輸入。</span><span class="sxs-lookup"><span data-stu-id="87ec6-127">Type `BlazorSignalRApp` in the **Project name** field.</span></span> <span data-ttu-id="87ec6-128">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="87ec6-129">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-129">Select **Create**.</span></span>

1. <span data-ttu-id="87ec6-130">選擇 [ \*\* Blazor WebAssembly 應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="87ec6-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="87ec6-131">在 [ **Advanced**] 底下，選取 [ **ASP.NET Core**裝載] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="87ec6-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="87ec6-132">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-132">Select **Create**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="87ec6-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87ec6-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="87ec6-134">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="87ec6-134">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="87ec6-135">在 Visual Studio Code 中，開啟應用程式的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="87ec6-135">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="87ec6-136">當對話方塊出現以新增資產以建立和偵錯工具時，請選取 **[是**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-136">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="87ec6-137">Visual Studio Code 會自動新增 `.vscode` 具有所產生檔案和檔案的資料夾 `launch.json` `tasks.json` 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-137">Visual Studio Code automatically adds the `.vscode` folder with generated `launch.json` and `tasks.json` files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="87ec6-138">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="87ec6-138">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="87ec6-139">安裝最新版本的[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) ，並執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="87ec6-139">Install the latest version of [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) and perform the following steps:</span></span>

1. <span data-ttu-id="87ec6-140">**File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-140">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="87ec6-141">在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-141">In the sidebar, select **Web and Console** > **App**.</span></span>

1. <span data-ttu-id="87ec6-142">選擇 [ \*\* Blazor WebAssembly 應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="87ec6-142">Choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="87ec6-143">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-143">Select **Next**.</span></span>

   <span data-ttu-id="87ec6-144">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="87ec6-144">Confirm the following configurations:</span></span>

   * <span data-ttu-id="87ec6-145">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-145">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="87ec6-146">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-146">**Authentication** set to **No Authentication**.</span></span>

   <span data-ttu-id="87ec6-147">選取 [**主控 ASP.NET Core** ] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="87ec6-147">Select the **ASP.NET Core Hosted** check box.</span></span>

   <span data-ttu-id="87ec6-148">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-148">Select **Next**.</span></span>

1. <span data-ttu-id="87ec6-149">在 [**專案名稱**] 欄位中，將應用程式命名為 `BlazorSignalRApp` 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-149">In the **Project Name** field, name the app `BlazorSignalRApp`.</span></span> <span data-ttu-id="87ec6-150">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-150">Select **Create**.</span></span>

   <span data-ttu-id="87ec6-151">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="87ec6-151">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="87ec6-152">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="87ec6-152">The user and keychain passwords are required to trust the certificate.</span></span>

1. <span data-ttu-id="87ec6-153">流覽至專案資料夾，然後開啟專案的方案檔（），以開啟專案 `.sln` 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-153">Open the project by navigating to the project folder and opening the project's solution file (`.sln`).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="87ec6-154">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="87ec6-154">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="87ec6-155">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="87ec6-155">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="87ec6-156">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="87ec6-156">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="87ec6-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87ec6-157">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="87ec6-158">在**方案總管**中，以滑鼠右鍵按一下 `BlazorSignalRApp.Client` 專案，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-158">In **Solution Explorer**, right-click the `BlazorSignalRApp.Client` project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="87ec6-159">在 [**管理 NuGet 封裝**] 對話方塊中，確認 [**套件來源**] 設定為 `nuget.org` 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-159">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to `nuget.org`.</span></span>

1. <span data-ttu-id="87ec6-160">在選取 **[流覽]** 的情況下，于 `Microsoft.AspNetCore.SignalR.Client` 搜尋方塊中輸入。</span><span class="sxs-lookup"><span data-stu-id="87ec6-160">With **Browse** selected, type `Microsoft.AspNetCore.SignalR.Client` in the search box.</span></span>

1. <span data-ttu-id="87ec6-161">在搜尋結果中選取套件， [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) 然後選取 [**安裝**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-161">In the search results, select the [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package and select **Install**.</span></span>

1. <span data-ttu-id="87ec6-162">如果出現 [**預覽變更**] 對話方塊，請選取 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="87ec6-162">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="87ec6-163">如果 [**接受授權**] 對話方塊出現，如果您同意授權條款，請選取 [**我接受**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-163">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="87ec6-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87ec6-164">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="87ec6-165">在**整合式終端**機（從工具列**觀看**  >  **終端**機）中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="87ec6-165">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="87ec6-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="87ec6-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="87ec6-167">在 [**解決方案**] 提要欄位中，以滑鼠右鍵按一下 `BlazorSignalRApp.Client` 專案，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-167">In the **Solution** sidebar, right-click the `BlazorSignalRApp.Client` project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="87ec6-168">在 [**管理 NuGet 封裝**] 對話方塊中，確認 [來源] 下拉式設定為 `nuget.org` 。</span><span class="sxs-lookup"><span data-stu-id="87ec6-168">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to `nuget.org`.</span></span>

1. <span data-ttu-id="87ec6-169">在選取 **[流覽]** 的情況下，于 `Microsoft.AspNetCore.SignalR.Client` 搜尋方塊中輸入。</span><span class="sxs-lookup"><span data-stu-id="87ec6-169">With **Browse** selected, type `Microsoft.AspNetCore.SignalR.Client` in the search box.</span></span>

1. <span data-ttu-id="87ec6-170">在搜尋結果中，選取封裝旁的核取方塊 [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) ，然後選取 [**新增套件**]。</span><span class="sxs-lookup"><span data-stu-id="87ec6-170">In the search results, select the check box next to the [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package and select **Add Package**.</span></span>

1. <span data-ttu-id="87ec6-171">如果 [**接受授權**] 對話方塊出現，請選取 [**接受**] （如果您同意授權條款）。</span><span class="sxs-lookup"><span data-stu-id="87ec6-171">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="87ec6-172">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="87ec6-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="87ec6-173">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="87ec6-173">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="87ec6-174">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="87ec6-174">Add a SignalR hub</span></span>

<span data-ttu-id="87ec6-175">在 `BlazorSignalRApp.Server` 專案中，建立 `Hubs` （複數）資料夾，並新增下列 `ChatHub` 類別（ `Hubs/ChatHub.cs` ）：</span><span class="sxs-lookup"><span data-stu-id="87ec6-175">In the `BlazorSignalRApp.Server` project, create a `Hubs` (plural) folder and add the following `ChatHub` class (`Hubs/ChatHub.cs`):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="87ec6-176">新增服務和中樞的端點 SignalR</span><span class="sxs-lookup"><span data-stu-id="87ec6-176">Add services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="87ec6-177">在 `BlazorSignalRApp.Server` 專案中，開啟 `Startup.cs` 檔案。</span><span class="sxs-lookup"><span data-stu-id="87ec6-177">In the `BlazorSignalRApp.Server` project, open the `Startup.cs` file.</span></span>

1. <span data-ttu-id="87ec6-178">將類別的命名空間新增 `ChatHub` 至檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="87ec6-178">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="87ec6-179">將 SignalR 和回應壓縮中介軟體服務新增至 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="87ec6-179">Add SignalR and Response Compression Middleware services to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_ConfigureServices&highlight=3,5-9)]

1. <span data-ttu-id="87ec6-180">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="87ec6-180">In `Startup.Configure`:</span></span>

   * <span data-ttu-id="87ec6-181">使用處理管線設定頂端的回應壓縮中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87ec6-181">Use Response Compression Middleware at the top of the processing pipeline's configuration.</span></span>
   * <span data-ttu-id="87ec6-182">在控制器的端點和用戶端的回退之間，新增中樞的端點。</span><span class="sxs-lookup"><span data-stu-id="87ec6-182">Between the endpoints for controllers and the client-side fallback, add an endpoint for the hub.</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_Configure&highlight=3,25)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="87ec6-183">新增 Razor 聊天的元件程式碼</span><span class="sxs-lookup"><span data-stu-id="87ec6-183">Add Razor component code for chat</span></span>

1. <span data-ttu-id="87ec6-184">在 `BlazorSignalRApp.Client` 專案中，開啟 `Pages/Index.razor` 檔案。</span><span class="sxs-lookup"><span data-stu-id="87ec6-184">In the `BlazorSignalRApp.Client` project, open the `Pages/Index.razor` file.</span></span>

1. <span data-ttu-id="87ec6-185">將標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="87ec6-185">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="87ec6-186">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="87ec6-186">Run the app</span></span>

1. <span data-ttu-id="87ec6-187">遵循您工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="87ec6-187">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="87ec6-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87ec6-188">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="87ec6-189">在 [**方案總管**中，選取 `BlazorSignalRApp.Server` 專案。</span><span class="sxs-lookup"><span data-stu-id="87ec6-189">In **Solution Explorer**, select the `BlazorSignalRApp.Server` project.</span></span> <span data-ttu-id="87ec6-190">按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd> + <kbd>F5</kbd> ，以執行應用程式而不進行偵測。</span><span class="sxs-lookup"><span data-stu-id="87ec6-190">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="87ec6-191">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="87ec6-191">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="87ec6-192">選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="87ec6-192">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="87ec6-193">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="87ec6-193">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="87ec6-194">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="87ec6-194">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="87ec6-195">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="87ec6-195">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="87ec6-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87ec6-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="87ec6-197">當 VS Code 提供來建立伺服器應用程式（）的啟動設定檔時 `.vscode/launch.json` ， `program` 專案會顯示如下，以指向應用程式的元件（ `{APPLICATION NAME}.Server.dll` ）：</span><span class="sxs-lookup"><span data-stu-id="87ec6-197">When VS Code offers to create a launch profile for the Server app (`.vscode/launch.json`), the `program` entry appears similar to the following to point to the app's assembly (`{APPLICATION NAME}.Server.dll`):</span></span>

   ```json
   "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/{APPLICATION NAME}.Server.dll"
   ```

1. <span data-ttu-id="87ec6-198">按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd> + <kbd>F5</kbd> ，以執行應用程式而不進行偵測。</span><span class="sxs-lookup"><span data-stu-id="87ec6-198">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="87ec6-199">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="87ec6-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="87ec6-200">選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="87ec6-200">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="87ec6-201">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="87ec6-201">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="87ec6-202">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="87ec6-202">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="87ec6-203">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="87ec6-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="87ec6-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="87ec6-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="87ec6-205">在 [**方案**] 提要欄位中，選取 `BlazorSignalRApp.Server` 專案。</span><span class="sxs-lookup"><span data-stu-id="87ec6-205">In the **Solution** sidebar, select the `BlazorSignalRApp.Server` project.</span></span> <span data-ttu-id="87ec6-206">按<kbd>則是⌘</kbd> + <kbd>↩</kbd>以執行應用程式的偵錯工具，或<kbd>⌥</kbd> + <kbd>則是⌘</kbd> + <kbd>↩</kbd>以執行應用程式而不進行偵測。</span><span class="sxs-lookup"><span data-stu-id="87ec6-206">Press <kbd>⌘</kbd>+<kbd>↩</kbd> to run the app with debugging or <kbd>⌥</kbd>+<kbd>⌘</kbd>+<kbd>↩</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="87ec6-207">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="87ec6-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="87ec6-208">選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="87ec6-208">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="87ec6-209">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="87ec6-209">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="87ec6-210">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="87ec6-210">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="87ec6-211">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="87ec6-211">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="87ec6-212">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="87ec6-212">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="87ec6-213">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="87ec6-213">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="87ec6-214">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="87ec6-214">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="87ec6-215">選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="87ec6-215">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="87ec6-216">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="87ec6-216">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="87ec6-217">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="87ec6-217">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="87ec6-218">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="87ec6-218">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="87ec6-219">後續步驟</span><span class="sxs-lookup"><span data-stu-id="87ec6-219">Next steps</span></span>

<span data-ttu-id="87ec6-220">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="87ec6-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="87ec6-221">建立 Blazor WebAssembly 託管應用程式專案</span><span class="sxs-lookup"><span data-stu-id="87ec6-221">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="87ec6-222">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="87ec6-222">Add the SignalR client library</span></span>
> * <span data-ttu-id="87ec6-223">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="87ec6-223">Add a SignalR hub</span></span>
> * <span data-ttu-id="87ec6-224">新增 SignalR 服務和中樞的端點 SignalR</span><span class="sxs-lookup"><span data-stu-id="87ec6-224">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="87ec6-225">新增 Razor 聊天的元件程式碼</span><span class="sxs-lookup"><span data-stu-id="87ec6-225">Add Razor component code for chat</span></span>

<span data-ttu-id="87ec6-226">若要深入瞭解如何建立 Blazor 應用程式，請參閱 Blazor 檔：</span><span class="sxs-lookup"><span data-stu-id="87ec6-226">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="87ec6-227">其他資源</span><span class="sxs-lookup"><span data-stu-id="87ec6-227">Additional resources</span></span>

* <xref:signalr/introduction>
* <span data-ttu-id="87ec6-228">[SignalR用於驗證的跨原始來源協調](xref:blazor/fundamentals/additional-scenarios#signalr-cross-origin-negotiation-for-authentication)</span><span class="sxs-lookup"><span data-stu-id="87ec6-228">[SignalR cross-origin negotiation for authentication](xref:blazor/fundamentals/additional-scenarios#signalr-cross-origin-negotiation-for-authentication)</span></span>
