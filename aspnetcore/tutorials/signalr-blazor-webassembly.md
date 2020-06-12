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
ms.openlocfilehash: 720f534426cc0e2b32778e49050c7f7d75ecd60d
ms.sourcegitcommit: 6371114344a5f4fbc5d4a119b0be1ad3762e0216
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2020
ms.locfileid: "84679588"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="726c7-103">搭配 WebAssembly 使用 ASP.NET Core SignalR Blazor</span><span class="sxs-lookup"><span data-stu-id="726c7-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="726c7-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="726c7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="726c7-105">本教學課程將教您使用 with WebAssembly 建立即時應用程式的基本概念 SignalR Blazor 。</span><span class="sxs-lookup"><span data-stu-id="726c7-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="726c7-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="726c7-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="726c7-107">建立 Blazor WebAssembly 託管應用程式專案</span><span class="sxs-lookup"><span data-stu-id="726c7-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="726c7-108">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="726c7-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="726c7-109">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="726c7-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="726c7-110">新增 SignalR 服務和中樞的端點 SignalR</span><span class="sxs-lookup"><span data-stu-id="726c7-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="726c7-111">新增 Razor 聊天的元件程式碼</span><span class="sxs-lookup"><span data-stu-id="726c7-111">Add Razor component code for chat</span></span>

<span data-ttu-id="726c7-112">在本教學課程結尾，您將會有一個可運作的聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="726c7-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="726c7-113">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="726c7-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="726c7-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="726c7-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="726c7-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="726c7-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="726c7-116">使用**ASP.NET 和 網頁程式開發**工作負載[Visual Studio 2019 16.6 或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="726c7-116">[Visual Studio 2019 16.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="726c7-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="726c7-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="726c7-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="726c7-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="726c7-119">Visual Studio for Mac 8.6 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="726c7-119">Visual Studio for Mac version 8.6 or later</span></span>](https://visualstudio.microsoft.com/vs/mac/)
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="726c7-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="726c7-120">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="726c7-121">建立 hosted Blazor WebAssembly 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="726c7-121">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="726c7-122">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="726c7-122">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="726c7-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="726c7-123">Visual Studio</span></span>](#tab/visual-studio)

> [!NOTE]
> <span data-ttu-id="726c7-124">Visual Studio 16.6 或更新版本，而且需要 .NET Core SDK 3.1.300 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="726c7-124">Visual Studio 16.6 or later and .NET Core SDK 3.1.300 or later are required.</span></span>

1. <span data-ttu-id="726c7-125">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="726c7-125">Create a new project.</span></span>

1. <span data-ttu-id="726c7-126">選取\*\* Blazor 應用程式\*\*，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="726c7-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="726c7-127">在 [**專案名稱**] 欄位中輸入 "BlazorSignalRApp"。</span><span class="sxs-lookup"><span data-stu-id="726c7-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="726c7-128">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="726c7-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="726c7-129">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="726c7-129">Select **Create**.</span></span>

1. <span data-ttu-id="726c7-130">選擇 [ \*\* Blazor WebAssembly 應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="726c7-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="726c7-131">在 [ **Advanced**] 底下，選取 [ **ASP.NET Core**裝載] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="726c7-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="726c7-132">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="726c7-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="726c7-133">如果您已升級或安裝新版本的 Visual Studio，而 Blazor WebAssembly 範本未出現在 VS UI 中，請使用 `dotnet new` 先前所示的命令重新安裝範本。</span><span class="sxs-lookup"><span data-stu-id="726c7-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="726c7-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="726c7-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="726c7-135">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="726c7-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="726c7-136">在 Visual Studio Code 中，開啟應用程式的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="726c7-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="726c7-137">當對話方塊出現以新增資產以建立和偵錯工具時，請選取 **[是**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="726c7-138">Visual Studio Code 會自動新增*vscode*資料夾，其中包含*在*檔案上產生的launch.js和*tasks.js* 。</span><span class="sxs-lookup"><span data-stu-id="726c7-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="726c7-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="726c7-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="726c7-140">安裝最新版本的[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) ，並執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="726c7-140">Install the latest version of [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) and perform the following steps:</span></span>

1. <span data-ttu-id="726c7-141">**File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。</span><span class="sxs-lookup"><span data-stu-id="726c7-141">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="726c7-142">在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-142">In the sidebar, select **Web and Console** > **App**.</span></span>

1. <span data-ttu-id="726c7-143">選擇 [ \*\* Blazor WebAssembly 應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="726c7-143">Choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="726c7-144">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="726c7-144">Select **Next**.</span></span>

   <span data-ttu-id="726c7-145">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="726c7-145">Confirm the following configurations:</span></span>

   * <span data-ttu-id="726c7-146">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="726c7-146">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="726c7-147">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-147">**Authentication** set to **No Authentication**.</span></span>

   <span data-ttu-id="726c7-148">選取 [**主控 ASP.NET Core** ] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="726c7-148">Select the **ASP.NET Core Hosted** check box.</span></span>

   <span data-ttu-id="726c7-149">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="726c7-149">Select **Next**.</span></span>

1. <span data-ttu-id="726c7-150">在 [**專案名稱**] 欄位中，將應用程式命名為 `BlazorSignalRApp` 。</span><span class="sxs-lookup"><span data-stu-id="726c7-150">In the **Project Name** field, name the app `BlazorSignalRApp`.</span></span> <span data-ttu-id="726c7-151">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="726c7-151">Select **Create**.</span></span>

   <span data-ttu-id="726c7-152">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="726c7-152">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="726c7-153">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="726c7-153">The user and keychain passwords are required to trust the certificate.</span></span>

1. <span data-ttu-id="726c7-154">流覽至專案資料夾，然後開啟專案的方案檔（*.sln*），以開啟專案。</span><span class="sxs-lookup"><span data-stu-id="726c7-154">Open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="726c7-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="726c7-155">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="726c7-156">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="726c7-156">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="726c7-157">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="726c7-157">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="726c7-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="726c7-158">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="726c7-159">在**方案總管**中，以滑鼠右鍵按一下 [ **BlazorSignalRApp** ] 專案，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-159">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="726c7-160">在 [**管理 NuGet 封裝**] 對話方塊中，確認 [**套件來源**] 已設定為 [ *nuget.org*]。</span><span class="sxs-lookup"><span data-stu-id="726c7-160">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="726c7-161">在選取 **[流覽]** 的情況下，輸入 "AspNetCore. SignalR .。用戶端：在搜尋方塊中。</span><span class="sxs-lookup"><span data-stu-id="726c7-161">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="726c7-162">在搜尋結果中，選取 [ [AspNetCore SignalR ]。用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)套件，然後選取 [**安裝**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-162">In the search results, select the [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) package and select **Install**.</span></span>

1. <span data-ttu-id="726c7-163">如果出現 [**預覽變更**] 對話方塊，請選取 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="726c7-163">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="726c7-164">如果 [**接受授權**] 對話方塊出現，如果您同意授權條款，請選取 [**我接受**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-164">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="726c7-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="726c7-165">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="726c7-166">在**整合式終端**機（從工具列**觀看**  >  **終端**機）中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="726c7-166">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="726c7-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="726c7-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="726c7-168">在 [**解決方案**] 提要欄位中，以滑鼠右鍵按一下 [ **BlazorSignalRApp** ] 專案，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-168">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="726c7-169">在 [**管理 NuGet 封裝**] 對話方塊中，確認 [來源] 下拉式設定為 [ *nuget.org*]。</span><span class="sxs-lookup"><span data-stu-id="726c7-169">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="726c7-170">在選取 **[流覽]** 的情況下，輸入 "AspNetCore. SignalR .。用戶端：在搜尋方塊中。</span><span class="sxs-lookup"><span data-stu-id="726c7-170">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="726c7-171">在搜尋結果中，選取 [AspNetCore] 旁的複選[ SignalR 框。用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)套件，然後選取 [**新增封裝**]。</span><span class="sxs-lookup"><span data-stu-id="726c7-171">In the search results, select the check box next to the [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) package and select **Add Package**.</span></span>

1. <span data-ttu-id="726c7-172">如果 [**接受授權**] 對話方塊出現，請選取 [**接受**] （如果您同意授權條款）。</span><span class="sxs-lookup"><span data-stu-id="726c7-172">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="726c7-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="726c7-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="726c7-174">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="726c7-174">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="726c7-175">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="726c7-175">Add a SignalR hub</span></span>

<span data-ttu-id="726c7-176">在**BlazorSignalRApp**專案中，建立*中樞*（複數）資料夾，並新增下列 `ChatHub` 類別（*hub/ChatHub .cs*）：</span><span class="sxs-lookup"><span data-stu-id="726c7-176">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="726c7-177">新增服務和中樞的端點 SignalR</span><span class="sxs-lookup"><span data-stu-id="726c7-177">Add services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="726c7-178">在**BlazorSignalRApp**專案中，開啟*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="726c7-178">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="726c7-179">將類別的命名空間新增 `ChatHub` 至檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="726c7-179">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="726c7-180">將 SignalR 和回應壓縮中介軟體服務新增至 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="726c7-180">Add SignalR and Response Compression Middleware services to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_ConfigureServices&highlight=3,5-9)]

1. <span data-ttu-id="726c7-181">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="726c7-181">In `Startup.Configure`:</span></span>

   * <span data-ttu-id="726c7-182">使用處理管線設定頂端的回應壓縮中介軟體。</span><span class="sxs-lookup"><span data-stu-id="726c7-182">Use Response Compression Middleware at the top of the processing pipeline's configuration.</span></span>
   * <span data-ttu-id="726c7-183">在控制器的端點和用戶端的回退之間，新增中樞的端點。</span><span class="sxs-lookup"><span data-stu-id="726c7-183">Between the endpoints for controllers and the client-side fallback, add an endpoint for the hub.</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_Configure&highlight=3,25)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="726c7-184">新增 Razor 聊天的元件程式碼</span><span class="sxs-lookup"><span data-stu-id="726c7-184">Add Razor component code for chat</span></span>

1. <span data-ttu-id="726c7-185">在 [ **BlazorSignalRApp** ] 專案中，開啟*Pages/Index. razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="726c7-185">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="726c7-186">將標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="726c7-186">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="726c7-187">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="726c7-187">Run the app</span></span>

1. <span data-ttu-id="726c7-188">遵循您工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="726c7-188">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="726c7-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="726c7-189">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="726c7-190">在 [**方案總管**中，選取 [ **BlazorSignalRApp** ] 專案。</span><span class="sxs-lookup"><span data-stu-id="726c7-190">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="726c7-191">按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd> + <kbd>F5</kbd> ，以執行應用程式而不進行偵測。</span><span class="sxs-lookup"><span data-stu-id="726c7-191">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="726c7-192">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="726c7-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="726c7-193">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="726c7-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="726c7-194">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="726c7-194">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="726c7-195">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="726c7-195">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="726c7-196">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="726c7-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="726c7-197">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="726c7-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="726c7-198">當 VS Code 提供來建立伺服器應用程式的啟動設定檔（*vscode/launch.js*）時， `program` 專案會顯示如下，以指向應用程式的元件（ `{APPLICATION NAME}.Server.dll` ）：</span><span class="sxs-lookup"><span data-stu-id="726c7-198">When VS Code offers to create a launch profile for the Server app (*.vscode/launch.json*), the `program` entry appears similar to the following to point to the app's assembly (`{APPLICATION NAME}.Server.dll`):</span></span>

   ```json
   "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/{APPLICATION NAME}.Server.dll"
   ```

1. <span data-ttu-id="726c7-199">按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd> + <kbd>F5</kbd> ，以執行應用程式而不進行偵測。</span><span class="sxs-lookup"><span data-stu-id="726c7-199">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="726c7-200">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="726c7-200">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="726c7-201">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="726c7-201">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="726c7-202">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="726c7-202">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="726c7-203">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="726c7-203">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="726c7-204">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="726c7-204">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="726c7-205">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="726c7-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="726c7-206">在 [**方案**] 提要欄位中，選取 [ **BlazorSignalRApp** ] 專案。</span><span class="sxs-lookup"><span data-stu-id="726c7-206">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="726c7-207">按<kbd>則是⌘</kbd> + <kbd>↩</kbd>以執行應用程式的偵錯工具，或<kbd>⌥</kbd> + <kbd>則是⌘</kbd> + <kbd>↩</kbd>以執行應用程式而不進行偵測。</span><span class="sxs-lookup"><span data-stu-id="726c7-207">Press <kbd>⌘</kbd>+<kbd>↩</kbd> to run the app with debugging or <kbd>⌥</kbd>+<kbd>⌘</kbd>+<kbd>↩</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="726c7-208">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="726c7-208">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="726c7-209">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="726c7-209">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="726c7-210">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="726c7-210">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="726c7-211">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="726c7-211">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="726c7-212">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="726c7-212">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="726c7-213">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="726c7-213">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="726c7-214">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="726c7-214">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="726c7-215">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="726c7-215">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="726c7-216">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="726c7-216">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="726c7-217">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="726c7-217">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="726c7-218">![SignalRBlazorWebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="726c7-218">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="726c7-219">引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="726c7-219">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="726c7-220">後續步驟</span><span class="sxs-lookup"><span data-stu-id="726c7-220">Next steps</span></span>

<span data-ttu-id="726c7-221">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="726c7-221">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="726c7-222">建立 Blazor WebAssembly 託管應用程式專案</span><span class="sxs-lookup"><span data-stu-id="726c7-222">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="726c7-223">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="726c7-223">Add the SignalR client library</span></span>
> * <span data-ttu-id="726c7-224">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="726c7-224">Add a SignalR hub</span></span>
> * <span data-ttu-id="726c7-225">新增 SignalR 服務和中樞的端點 SignalR</span><span class="sxs-lookup"><span data-stu-id="726c7-225">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="726c7-226">新增 Razor 聊天的元件程式碼</span><span class="sxs-lookup"><span data-stu-id="726c7-226">Add Razor component code for chat</span></span>

<span data-ttu-id="726c7-227">若要深入瞭解如何建立 Blazor 應用程式，請參閱 Blazor 檔：</span><span class="sxs-lookup"><span data-stu-id="726c7-227">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="726c7-228">其他資源</span><span class="sxs-lookup"><span data-stu-id="726c7-228">Additional resources</span></span>

* <xref:signalr/introduction>
* <span data-ttu-id="726c7-229">[SignalR用於驗證的跨原始來源協調](xref:blazor/hosting-model-configuration#signalr-cross-origin-negotiation-for-authentication)</span><span class="sxs-lookup"><span data-stu-id="726c7-229">[SignalR cross-origin negotiation for authentication](xref:blazor/hosting-model-configuration#signalr-cross-origin-negotiation-for-authentication)</span></span>
