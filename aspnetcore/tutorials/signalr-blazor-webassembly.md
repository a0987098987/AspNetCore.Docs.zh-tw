---
title: 將ASP.NET核心SignalR與BlazorWeb 組裝一起使用
author: guardrex
description: 創建使用 ASP.NETSignalR核心Blazor與 Web 組裝的聊天應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/21/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: 03db8b48bdacec1d6877a4ea09f97c242761c42d
ms.sourcegitcommit: f976dce28ad887bbd31720c318fd4a97cf96cc6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81738015"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="2c8db-103">將ASP.NET核心信號R與布拉佐爾網路組裝一起使用</span><span class="sxs-lookup"><span data-stu-id="2c8db-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="2c8db-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2c8db-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2c8db-105">本教學將介紹使用 SignalR 使用 Blazor WebAssembly 構建即時應用程式的基礎知識。</span><span class="sxs-lookup"><span data-stu-id="2c8db-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="2c8db-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="2c8db-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c8db-107">建立 Blazor WebAssembly 託管應用專案</span><span class="sxs-lookup"><span data-stu-id="2c8db-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="2c8db-108">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="2c8db-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="2c8db-109">新增訊號R中心</span><span class="sxs-lookup"><span data-stu-id="2c8db-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="2c8db-110">SignalR 中心加入 SignalR 服務和終結點</span><span class="sxs-lookup"><span data-stu-id="2c8db-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="2c8db-111">新增用於聊天的 Razor 元件碼</span><span class="sxs-lookup"><span data-stu-id="2c8db-111">Add Razor component code for chat</span></span>

<span data-ttu-id="2c8db-112">在本教程結束時,您將有一個工作聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c8db-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="2c8db-113">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c8db-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c8db-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="2c8db-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2c8db-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8db-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="2c8db-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c8db-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2c8db-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2c8db-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="2c8db-118">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2c8db-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="2c8db-119">建立託管的 Blazor WebAssembly 應用專案</span><span class="sxs-lookup"><span data-stu-id="2c8db-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="2c8db-120">不使用 Visual Studio 版本 16.6 預覽版或更高版本時,請安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。</span><span class="sxs-lookup"><span data-stu-id="2c8db-120">When not using Visual Studio version 16.6 Preview 2 or later, install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="2c8db-121">[微軟.AspNetCore.元件.WebAssembly.範本](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/)包有預覽版本,而Blazor WebAssembly處於預覽狀態。</span><span class="sxs-lookup"><span data-stu-id="2c8db-121">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="2c8db-122">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview4.20210.8
```

<span data-ttu-id="2c8db-123">遵循您選擇工具的指南:</span><span class="sxs-lookup"><span data-stu-id="2c8db-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2c8db-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8db-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2c8db-125">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="2c8db-125">Create a new project.</span></span>

1. <span data-ttu-id="2c8db-126">選擇**Blazor 應用程式**並選擇 **「下一步**」。</span><span class="sxs-lookup"><span data-stu-id="2c8db-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="2c8db-127">在**專案名稱**欄位中鍵入"BlazorSignalRApp"</span><span class="sxs-lookup"><span data-stu-id="2c8db-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="2c8db-128">確認 **「位置**」條目正確或為專案提供位置。</span><span class="sxs-lookup"><span data-stu-id="2c8db-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2c8db-129">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2c8db-129">Select **Create**.</span></span>

1. <span data-ttu-id="2c8db-130">選擇**Blazor Web 組裝應用**範本。</span><span class="sxs-lookup"><span data-stu-id="2c8db-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="2c8db-131">在 **「進階」** 下,選擇 **「ASP.NET核心託管**」複選框。</span><span class="sxs-lookup"><span data-stu-id="2c8db-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="2c8db-132">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2c8db-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="2c8db-133">如果升級或安裝了新版本的 Visual Studio,並且 Blazor WebAssembly 範本未顯示在 VS`dotnet new`UI 中,請使用前面顯示 的命令重新安裝範本。</span><span class="sxs-lookup"><span data-stu-id="2c8db-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2c8db-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c8db-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2c8db-135">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="2c8db-136">在可視化工作室代碼中,打開應用的項目資料夾。</span><span class="sxs-lookup"><span data-stu-id="2c8db-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="2c8db-137">當對話框顯示為添加要生成和調試應用的資產時,選擇 **"是**"。</span><span class="sxs-lookup"><span data-stu-id="2c8db-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="2c8db-138">可視化工作室代碼自動添加 *.vscode*資料夾與生成的*啟動.json*和*任務.json*檔。</span><span class="sxs-lookup"><span data-stu-id="2c8db-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2c8db-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2c8db-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2c8db-140">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="2c8db-141">在 Mac 的 Visual Studio 中,透過導航到專案資料夾並打開專案的解決方案檔 *(.sln*) 來打開專案。</span><span class="sxs-lookup"><span data-stu-id="2c8db-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2c8db-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2c8db-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="2c8db-143">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="2c8db-144">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="2c8db-144">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2c8db-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8db-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="2c8db-146">在**解決方案資源管理器**中,右鍵單擊**BlazorSignalRApp.Client**專案,然後選擇 **「管理 NuGet 包**」。</span><span class="sxs-lookup"><span data-stu-id="2c8db-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="2c8db-147">在 **'管理 NuGet 套件'** 對話框中,確認**套件來源**設定為*nuget.org*。</span><span class="sxs-lookup"><span data-stu-id="2c8db-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="2c8db-148">選中 **「瀏覽**」後,在搜尋框中鍵入「Microsoft.AspNetCore.SignalR.用戶端」。。</span><span class="sxs-lookup"><span data-stu-id="2c8db-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="2c8db-149">在搜尋結果中,選擇`Microsoft.AspNetCore.SignalR.Client`包並選擇 **「安裝**」。</span><span class="sxs-lookup"><span data-stu-id="2c8db-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="2c8db-150">如果出現 **「預覽更改**」對話框,請選擇 **「確定**」。</span><span class="sxs-lookup"><span data-stu-id="2c8db-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="2c8db-151">如果出現 **「許可證接受」** 對話方塊,請選擇 **「我同意」** 許可條款。</span><span class="sxs-lookup"><span data-stu-id="2c8db-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2c8db-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c8db-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="2c8db-153">在**整合式終端**機 (從**工具列檢視** > **終端**)中,執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2c8db-154">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2c8db-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2c8db-155">在 **「解決方案**」側邊欄中,右鍵單擊**BlazorSignalRApp.用戶端**專案,然後選擇 **「管理 NuGet 包**」。</span><span class="sxs-lookup"><span data-stu-id="2c8db-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="2c8db-156">在 **'管理 NuGet 套件'** 對話框中,確認來源下拉清單設定為*nuget.org*。</span><span class="sxs-lookup"><span data-stu-id="2c8db-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="2c8db-157">選中 **「瀏覽**」後,在搜尋框中鍵入「Microsoft.AspNetCore.SignalR.用戶端」。。</span><span class="sxs-lookup"><span data-stu-id="2c8db-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="2c8db-158">在搜尋結果中,選擇`Microsoft.AspNetCore.SignalR.Client`包旁邊的複選框,然後選擇"**添加包**"。</span><span class="sxs-lookup"><span data-stu-id="2c8db-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="2c8db-159">如果出現 **「許可證接受」** 對話方塊,請選擇「如果同意許可條款,**則接受**」。。</span><span class="sxs-lookup"><span data-stu-id="2c8db-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2c8db-160">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2c8db-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="2c8db-161">在命令外殼中,執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="2c8db-162">新增訊號R中心</span><span class="sxs-lookup"><span data-stu-id="2c8db-162">Add a SignalR hub</span></span>

<span data-ttu-id="2c8db-163">在**BlazorSignalRApp.Server**專案中,建立*一個集線器*(`ChatHub`複數) 資料夾並新增以下類(*集線器/ ChatHub.cs):*</span><span class="sxs-lookup"><span data-stu-id="2c8db-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="2c8db-164">為 SignalR 中心新增服務和終結點</span><span class="sxs-lookup"><span data-stu-id="2c8db-164">Add services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="2c8db-165">在**BlazorSignalRApp.Server**專案中,打開*Startup.cs*檔。</span><span class="sxs-lookup"><span data-stu-id="2c8db-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="2c8db-166">將類別的`ChatHub`命名空間加入檔案的頂部:</span><span class="sxs-lookup"><span data-stu-id="2c8db-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="2c8db-167">將訊號 R 和回應壓縮中間元件`Startup.ConfigureServices`服務加入 :</span><span class="sxs-lookup"><span data-stu-id="2c8db-167">Add SignalR and Response Compression Middleware services to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_ConfigureServices&highlight=3,5-9)]

1. <span data-ttu-id="2c8db-168">在`Startup.Configure`控制器的終結點和客戶端回退之間,為集線器添加終結點:</span><span class="sxs-lookup"><span data-stu-id="2c8db-168">In `Startup.Configure` between the endpoints for controllers and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_UseEndpoints&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="2c8db-169">新增用於聊天的 Razor 元件碼</span><span class="sxs-lookup"><span data-stu-id="2c8db-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="2c8db-170">在**BlazorSignalRApp.Client**專案中,打開*頁面/Index.razor*檔。</span><span class="sxs-lookup"><span data-stu-id="2c8db-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="2c8db-171">將標記取代為以下代碼:</span><span class="sxs-lookup"><span data-stu-id="2c8db-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="2c8db-172">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="2c8db-172">Run the app</span></span>

1. <span data-ttu-id="2c8db-173">依工具指南操作:</span><span class="sxs-lookup"><span data-stu-id="2c8db-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2c8db-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8db-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2c8db-175">在**解決方案資源管理器**中,選擇**BlazorSignalRApp.Server**專案。</span><span class="sxs-lookup"><span data-stu-id="2c8db-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="2c8db-176">按<kbd>F5</kbd>以運行具有調試的應用,或<kbd>按 Ctrl</kbd>+<kbd>F5</kbd>運行應用而不進行調試。</span><span class="sxs-lookup"><span data-stu-id="2c8db-176">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="2c8db-177">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="2c8db-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2c8db-178">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2c8db-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2c8db-179">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="2c8db-179">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2c8db-181">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2c8db-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2c8db-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c8db-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2c8db-183">當 VS Code 提供為伺服器應用 *(.vscode/launch.json)* 建立啟動`program`設定檔時,該項目將顯示類似於以下內容以指向應用程式集`{APPLICATION NAME}.Server.dll`():</span><span class="sxs-lookup"><span data-stu-id="2c8db-183">When VS Code offers to create a launch profile for the Server app (*.vscode/launch.json*), the `program` entry appears similar to the following to point to the app's assembly (`{APPLICATION NAME}.Server.dll`):</span></span>

   ```json
   "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/{APPLICATION NAME}.Server.dll"
   ```

1. <span data-ttu-id="2c8db-184">按<kbd>F5</kbd>以運行具有調試的應用,或<kbd>按 Ctrl</kbd>+<kbd>F5</kbd>運行應用而不進行調試。</span><span class="sxs-lookup"><span data-stu-id="2c8db-184">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="2c8db-185">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="2c8db-185">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2c8db-186">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2c8db-186">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2c8db-187">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="2c8db-187">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2c8db-189">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2c8db-189">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2c8db-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2c8db-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2c8db-191">在 **「解決方案**」側邊欄中,選擇**BlazorSignalRApp.Server**專案。</span><span class="sxs-lookup"><span data-stu-id="2c8db-191">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="2c8db-192">按<kbd>[</kbd>+<kbd>↩</kbd>] 以執行應用程式與<kbd>⌥</kbd>+除錯或[ <kbd>↩</kbd>+<kbd>↩</kbd>執行應用程式而不進行除錯。</span><span class="sxs-lookup"><span data-stu-id="2c8db-192">Press <kbd>⌘</kbd>+<kbd>↩</kbd>\*\* to run the app with debugging or <kbd>⌥</kbd>+<kbd>⌘</kbd>+<kbd>↩</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="2c8db-193">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="2c8db-193">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2c8db-194">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2c8db-194">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2c8db-195">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="2c8db-195">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2c8db-197">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2c8db-197">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2c8db-198">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2c8db-198">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="2c8db-199">在命令外殼中,執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="2c8db-199">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="2c8db-200">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="2c8db-200">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2c8db-201">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2c8db-201">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2c8db-202">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="2c8db-202">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2c8db-204">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2c8db-204">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="2c8db-205">後續步驟</span><span class="sxs-lookup"><span data-stu-id="2c8db-205">Next steps</span></span>

<span data-ttu-id="2c8db-206">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="2c8db-206">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c8db-207">建立BlazorWeb 群組載託管應用專案</span><span class="sxs-lookup"><span data-stu-id="2c8db-207">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="2c8db-208">新增SignalR用戶端函式庫</span><span class="sxs-lookup"><span data-stu-id="2c8db-208">Add the SignalR client library</span></span>
> * <span data-ttu-id="2c8db-209">新增SignalR中心</span><span class="sxs-lookup"><span data-stu-id="2c8db-209">Add a SignalR hub</span></span>
> * <span data-ttu-id="2c8db-210">為SignalRSignalR中心新增服務和終結點</span><span class="sxs-lookup"><span data-stu-id="2c8db-210">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="2c8db-211">新增用於聊天的 Razor 元件碼</span><span class="sxs-lookup"><span data-stu-id="2c8db-211">Add Razor component code for chat</span></span>

<span data-ttu-id="2c8db-212">要瞭解有關建Blazor置 應用程式的資訊Blazor, 請參考以下文件:</span><span class="sxs-lookup"><span data-stu-id="2c8db-212">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="2c8db-213">其他資源</span><span class="sxs-lookup"><span data-stu-id="2c8db-213">Additional resources</span></span>

* <xref:signalr/introduction>
