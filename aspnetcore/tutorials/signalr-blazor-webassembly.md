---
title: 使用 ASP.NET Core SignalR 搭配 Blazor WebAssembly
author: guardrex
description: 建立使用 ASP.NET Core SignalR 搭配 Blazor WebAssembly 的聊天應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: c4843dc282e1978b39738e206ecc79ded87fcff9
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306577"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="12b39-103">搭配使用 ASP.NET Core SignalR 與 Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="12b39-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="12b39-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="12b39-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="12b39-105">本教學課程將教您使用 SignalR 搭配 Blazor WebAssembly 建立即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="12b39-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="12b39-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="12b39-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12b39-107">建立 Blazor WebAssembly 託管應用程式專案</span><span class="sxs-lookup"><span data-stu-id="12b39-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="12b39-108">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="12b39-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="12b39-109">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="12b39-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="12b39-110">新增 SignalR 服務和 SignalR 中樞的端點</span><span class="sxs-lookup"><span data-stu-id="12b39-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="12b39-111">新增 Razor 元件程式碼以進行聊天</span><span class="sxs-lookup"><span data-stu-id="12b39-111">Add Razor component code for chat</span></span>

<span data-ttu-id="12b39-112">在本教學課程結尾，您將會有一個可運作的聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="12b39-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="12b39-113">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12b39-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12b39-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="12b39-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="12b39-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12b39-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="12b39-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12b39-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="12b39-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="12b39-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="12b39-118">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12b39-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="12b39-119">建立 hosted Blazor WebAssembly 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="12b39-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="12b39-120">當不使用 Visual Studio 16.6 版 Preview 2 或更新版本時，請安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。</span><span class="sxs-lookup"><span data-stu-id="12b39-120">When not using Visual Studio version 16.6 Preview 2 or later, install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="12b39-121">Blazor WebAssembly 處於預覽階段時， [WebAssembly](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/)套件會有預覽版本。</span><span class="sxs-lookup"><span data-stu-id="12b39-121">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="12b39-122">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
```

<span data-ttu-id="12b39-123">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="12b39-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="12b39-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12b39-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="12b39-125">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="12b39-125">Create a new project.</span></span>

1. <span data-ttu-id="12b39-126">選取 [ **Blazor 應用程式**] 並選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="12b39-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="12b39-127">在 [**專案名稱**] 欄位中輸入 "BlazorSignalRApp"。</span><span class="sxs-lookup"><span data-stu-id="12b39-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="12b39-128">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="12b39-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="12b39-129">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="12b39-129">Select **Create**.</span></span>

1. <span data-ttu-id="12b39-130">選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="12b39-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="12b39-131">在 [ **Advanced**] 底下，選取 [ **ASP.NET Core**裝載] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="12b39-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="12b39-132">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="12b39-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="12b39-133">如果您已升級或安裝新版 Visual Studio，且 VS UI 中未出現 Blazor WebAssembly 範本，請使用先前所示的 `dotnet new` 命令重新安裝範本。</span><span class="sxs-lookup"><span data-stu-id="12b39-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="12b39-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12b39-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="12b39-135">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="12b39-136">在 Visual Studio Code 中，開啟應用程式的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="12b39-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="12b39-137">當對話方塊出現以新增資產以建立和偵錯工具時，請選取 **[是**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="12b39-138">Visual Studio Code 會自動新增*vscode*資料夾，其中包含產生的*啟動 json*和工作 *. json*檔案。</span><span class="sxs-lookup"><span data-stu-id="12b39-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="12b39-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="12b39-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="12b39-140">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="12b39-141">在 Visual Studio for Mac 中，流覽至專案資料夾，然後開啟專案的方案檔（ *.sln*），以開啟專案。</span><span class="sxs-lookup"><span data-stu-id="12b39-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="12b39-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12b39-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="12b39-143">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="12b39-144">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="12b39-144">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="12b39-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12b39-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="12b39-146">在**方案總管**中，以滑鼠右鍵按一下 [ **BlazorSignalRApp** ] 專案，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="12b39-147">在 [**管理 NuGet 封裝**] 對話方塊中，確認 [**套件來源**] 已設定為 [ *nuget.org*]。</span><span class="sxs-lookup"><span data-stu-id="12b39-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="12b39-148">在選取 **[流覽]** 的情況下，于搜尋方塊中輸入 "AspNetCore. SignalR. Client"。</span><span class="sxs-lookup"><span data-stu-id="12b39-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="12b39-149">在搜尋結果中，選取 [`Microsoft.AspNetCore.SignalR.Client`] 套件，然後選取 [**安裝**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="12b39-150">如果出現 [**預覽變更**] 對話方塊，請選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="12b39-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="12b39-151">如果 [**接受授權**] 對話方塊出現，如果您同意授權條款，請選取 [**我接受**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="12b39-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12b39-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="12b39-153">在**整合式終端**機（從工具列中的 [**View** > **終端**機]）中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="12b39-154">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="12b39-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="12b39-155">在 [**解決方案**] 提要欄位中，以滑鼠右鍵按一下 [ **BlazorSignalRApp** ] 專案，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="12b39-156">在 [**管理 NuGet 封裝**] 對話方塊中，確認 [來源] 下拉式設定為 [ *nuget.org*]。</span><span class="sxs-lookup"><span data-stu-id="12b39-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="12b39-157">在選取 **[流覽]** 的情況下，于搜尋方塊中輸入 "AspNetCore. SignalR. Client"。</span><span class="sxs-lookup"><span data-stu-id="12b39-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="12b39-158">在搜尋結果中，選取 `Microsoft.AspNetCore.SignalR.Client` 封裝旁的核取方塊，然後選取 [**新增套件**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="12b39-159">如果 [**接受授權**] 對話方塊出現，請選取 [**接受**] （如果您同意授權條款）。</span><span class="sxs-lookup"><span data-stu-id="12b39-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="12b39-160">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12b39-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="12b39-161">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="12b39-162">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="12b39-162">Add a SignalR hub</span></span>

<span data-ttu-id="12b39-163">在**BlazorSignalRApp**專案中，建立*中樞*（複數）資料夾，並新增下列 `ChatHub` 類別（*hub/ChatHub .cs*）：</span><span class="sxs-lookup"><span data-stu-id="12b39-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="12b39-164">新增 SignalR 服務和 SignalR 中樞的端點</span><span class="sxs-lookup"><span data-stu-id="12b39-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="12b39-165">在**BlazorSignalRApp**專案中，開啟*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="12b39-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="12b39-166">將 `ChatHub` 類別的命名空間新增至檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="12b39-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="12b39-167">將 SignalR 服務新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="12b39-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="12b39-168">在 [預設控制器路由] 和 [用戶端回溯] 的端點之間 `Startup.Configure` 中，新增中樞的端點：</span><span class="sxs-lookup"><span data-stu-id="12b39-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="12b39-169">新增 Razor 元件程式碼以進行聊天</span><span class="sxs-lookup"><span data-stu-id="12b39-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="12b39-170">在 [ **BlazorSignalRApp** ] 專案中，開啟*Pages/Index. razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="12b39-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="12b39-171">將標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="12b39-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="12b39-172">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="12b39-172">Run the app</span></span>

1. <span data-ttu-id="12b39-173">遵循您工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="12b39-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="12b39-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12b39-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="12b39-175">在 **方案總管**中，選取  **BlazorSignalRApp**  專案。</span><span class="sxs-lookup"><span data-stu-id="12b39-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="12b39-176">按**Ctrl + F5**執行應用程式，而不進行任何偵測。</span><span class="sxs-lookup"><span data-stu-id="12b39-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="12b39-177">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="12b39-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="12b39-178">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="12b39-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="12b39-179">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="12b39-179">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="12b39-181">引號：*星星 TREK VI：未探索的國家/地區*&copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="12b39-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="12b39-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12b39-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="12b39-183">選取 [ **Debug** > **執行但不**從工具列進行調試]。</span><span class="sxs-lookup"><span data-stu-id="12b39-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="12b39-184">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="12b39-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="12b39-185">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="12b39-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="12b39-186">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="12b39-186">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="12b39-188">引號：*星星 TREK VI：未探索的國家/地區*&copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="12b39-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="12b39-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="12b39-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="12b39-190">在 [**方案**] 提要欄位中，選取 [ **BlazorSignalRApp** ] 專案。</span><span class="sxs-lookup"><span data-stu-id="12b39-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="12b39-191">從功能表中，選取 [**執行**] > [**啟動但不進行調試**]。</span><span class="sxs-lookup"><span data-stu-id="12b39-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="12b39-192">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="12b39-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="12b39-193">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="12b39-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="12b39-194">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="12b39-194">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="12b39-196">引號：*星星 TREK VI：未探索的國家/地區*&copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="12b39-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="12b39-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12b39-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="12b39-198">在命令 shell 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12b39-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="12b39-199">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="12b39-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="12b39-200">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="12b39-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="12b39-201">名稱和訊息會立即顯示在兩個頁面上：</span><span class="sxs-lookup"><span data-stu-id="12b39-201">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="12b39-203">引號：*星星 TREK VI：未探索的國家/地區*&copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="12b39-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="12b39-204">後續步驟</span><span class="sxs-lookup"><span data-stu-id="12b39-204">Next steps</span></span>

<span data-ttu-id="12b39-205">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="12b39-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12b39-206">建立 Blazor WebAssembly 託管的應用程式專案</span><span class="sxs-lookup"><span data-stu-id="12b39-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="12b39-207">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="12b39-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="12b39-208">新增 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="12b39-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="12b39-209">新增 SignalR 服務和 SignalR 中樞的端點</span><span class="sxs-lookup"><span data-stu-id="12b39-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="12b39-210">新增 Razor 元件程式碼以進行聊天</span><span class="sxs-lookup"><span data-stu-id="12b39-210">Add Razor component code for chat</span></span>

<span data-ttu-id="12b39-211">若要深入瞭解如何建立 Blazor 應用程式，請參閱 Blazor 檔：</span><span class="sxs-lookup"><span data-stu-id="12b39-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="12b39-212">其他資源</span><span class="sxs-lookup"><span data-stu-id="12b39-212">Additional resources</span></span>

* <xref:signalr/introduction>
