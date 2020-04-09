---
title: 將ASP.NET核心SignalR與BlazorWeb 組裝一起使用
author: guardrex
description: 創建使用 ASP.NETSignalR核心Blazor與 Web 組裝的聊天應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: c4843dc282e1978b39738e206ecc79ded87fcff9
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80306577"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="bbae6-103">將ASP.NET核心信號R與布拉佐爾網路組裝一起使用</span><span class="sxs-lookup"><span data-stu-id="bbae6-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="bbae6-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bbae6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="bbae6-105">本教學將介紹使用 SignalR 使用 Blazor WebAssembly 構建即時應用程式的基礎知識。</span><span class="sxs-lookup"><span data-stu-id="bbae6-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="bbae6-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="bbae6-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbae6-107">建立 Blazor WebAssembly 託管應用專案</span><span class="sxs-lookup"><span data-stu-id="bbae6-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="bbae6-108">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="bbae6-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="bbae6-109">新增訊號R中心</span><span class="sxs-lookup"><span data-stu-id="bbae6-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="bbae6-110">SignalR 中心加入 SignalR 服務和終結點</span><span class="sxs-lookup"><span data-stu-id="bbae6-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="bbae6-111">新增用於聊天的 Razor 元件碼</span><span class="sxs-lookup"><span data-stu-id="bbae6-111">Add Razor component code for chat</span></span>

<span data-ttu-id="bbae6-112">在本教程結束時,您將有一個工作聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="bbae6-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="bbae6-113">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bbae6-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbae6-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="bbae6-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbae6-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbae6-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbae6-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbae6-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbae6-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bbae6-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="bbae6-118">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbae6-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="bbae6-119">建立託管的 Blazor WebAssembly 應用專案</span><span class="sxs-lookup"><span data-stu-id="bbae6-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="bbae6-120">不使用 Visual Studio 版本 16.6 預覽版或更高版本時,請安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。</span><span class="sxs-lookup"><span data-stu-id="bbae6-120">When not using Visual Studio version 16.6 Preview 2 or later, install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="bbae6-121">[微軟.AspNetCore.元件.WebAssembly.範本](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/)包有預覽版本,而Blazor WebAssembly處於預覽狀態。</span><span class="sxs-lookup"><span data-stu-id="bbae6-121">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="bbae6-122">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
```

<span data-ttu-id="bbae6-123">遵循您選擇工具的指南:</span><span class="sxs-lookup"><span data-stu-id="bbae6-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbae6-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbae6-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="bbae6-125">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="bbae6-125">Create a new project.</span></span>

1. <span data-ttu-id="bbae6-126">選擇**Blazor 應用程式**並選擇 **「下一步**」。</span><span class="sxs-lookup"><span data-stu-id="bbae6-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="bbae6-127">在**專案名稱**欄位中鍵入"BlazorSignalRApp"</span><span class="sxs-lookup"><span data-stu-id="bbae6-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="bbae6-128">確認 **「位置**」條目正確或為專案提供位置。</span><span class="sxs-lookup"><span data-stu-id="bbae6-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="bbae6-129">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="bbae6-129">Select **Create**.</span></span>

1. <span data-ttu-id="bbae6-130">選擇**Blazor Web 組裝應用**範本。</span><span class="sxs-lookup"><span data-stu-id="bbae6-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="bbae6-131">在 **「進階」** 下,選擇 **「ASP.NET核心託管**」複選框。</span><span class="sxs-lookup"><span data-stu-id="bbae6-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="bbae6-132">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="bbae6-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="bbae6-133">如果升級或安裝了新版本的 Visual Studio,並且 Blazor WebAssembly 範本未顯示在 VS`dotnet new`UI 中,請使用前面顯示 的命令重新安裝範本。</span><span class="sxs-lookup"><span data-stu-id="bbae6-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbae6-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbae6-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="bbae6-135">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="bbae6-136">在可視化工作室代碼中,打開應用的項目資料夾。</span><span class="sxs-lookup"><span data-stu-id="bbae6-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="bbae6-137">當對話框顯示為添加要生成和調試應用的資產時,選擇 **"是**"。</span><span class="sxs-lookup"><span data-stu-id="bbae6-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="bbae6-138">可視化工作室代碼自動添加 *.vscode*資料夾與生成的*啟動.json*和*任務.json*檔。</span><span class="sxs-lookup"><span data-stu-id="bbae6-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbae6-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bbae6-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="bbae6-140">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="bbae6-141">在 Mac 的 Visual Studio 中,透過導航到專案資料夾並打開專案的解決方案檔 *(.sln*) 來打開專案。</span><span class="sxs-lookup"><span data-stu-id="bbae6-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="bbae6-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbae6-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="bbae6-143">在命令 shell 中,執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="bbae6-144">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="bbae6-144">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbae6-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbae6-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="bbae6-146">在**解決方案資源管理器**中,右鍵單擊**BlazorSignalRApp.Client**專案,然後選擇 **「管理 NuGet 包**」。</span><span class="sxs-lookup"><span data-stu-id="bbae6-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="bbae6-147">在 **'管理 NuGet 套件'** 對話框中,確認**套件來源**設定為*nuget.org*。</span><span class="sxs-lookup"><span data-stu-id="bbae6-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="bbae6-148">選中 **「瀏覽**」後,在搜尋框中鍵入「Microsoft.AspNetCore.SignalR.用戶端」。。</span><span class="sxs-lookup"><span data-stu-id="bbae6-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="bbae6-149">在搜尋結果中,選擇`Microsoft.AspNetCore.SignalR.Client`包並選擇 **「安裝**」。</span><span class="sxs-lookup"><span data-stu-id="bbae6-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="bbae6-150">如果出現 **「預覽更改**」對話框,請選擇 **「確定**」。</span><span class="sxs-lookup"><span data-stu-id="bbae6-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="bbae6-151">如果出現 **「許可證接受」** 對話方塊,請選擇 **「我同意」** 許可條款。</span><span class="sxs-lookup"><span data-stu-id="bbae6-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbae6-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbae6-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="bbae6-153">在**整合式終端**機 (從**工具列檢視** > **終端**)中,執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbae6-154">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bbae6-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="bbae6-155">在 **「解決方案**」側邊欄中,右鍵單擊**BlazorSignalRApp.用戶端**專案,然後選擇 **「管理 NuGet 包**」。</span><span class="sxs-lookup"><span data-stu-id="bbae6-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="bbae6-156">在 **'管理 NuGet 套件'** 對話框中,確認來源下拉清單設定為*nuget.org*。</span><span class="sxs-lookup"><span data-stu-id="bbae6-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="bbae6-157">選中 **「瀏覽**」後,在搜尋框中鍵入「Microsoft.AspNetCore.SignalR.用戶端」。。</span><span class="sxs-lookup"><span data-stu-id="bbae6-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="bbae6-158">在搜尋結果中,選擇`Microsoft.AspNetCore.SignalR.Client`包旁邊的複選框,然後選擇"**添加包**"。</span><span class="sxs-lookup"><span data-stu-id="bbae6-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="bbae6-159">如果出現 **「許可證接受」** 對話方塊,請選擇「如果同意許可條款,**則接受**」。。</span><span class="sxs-lookup"><span data-stu-id="bbae6-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="bbae6-160">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbae6-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="bbae6-161">在命令外殼中,執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="bbae6-162">新增訊號R中心</span><span class="sxs-lookup"><span data-stu-id="bbae6-162">Add a SignalR hub</span></span>

<span data-ttu-id="bbae6-163">在**BlazorSignalRApp.Server**專案中,建立*一個集線器*(`ChatHub`複數) 資料夾並新增以下類(*集線器/ ChatHub.cs):*</span><span class="sxs-lookup"><span data-stu-id="bbae6-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="bbae6-164">SignalR 中心加入 SignalR 服務和終結點</span><span class="sxs-lookup"><span data-stu-id="bbae6-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="bbae6-165">在**BlazorSignalRApp.Server**專案中,打開*Startup.cs*檔。</span><span class="sxs-lookup"><span data-stu-id="bbae6-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="bbae6-166">將類別的`ChatHub`命名空間加入檔案的頂部:</span><span class="sxs-lookup"><span data-stu-id="bbae6-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="bbae6-167">新增 SignalR`Startup.ConfigureServices`服務</span><span class="sxs-lookup"><span data-stu-id="bbae6-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="bbae6-168">在`Startup.Configure`預設控制器路由的終結點和用戶端回退之間,為中心添加終結點:</span><span class="sxs-lookup"><span data-stu-id="bbae6-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="bbae6-169">新增用於聊天的 Razor 元件碼</span><span class="sxs-lookup"><span data-stu-id="bbae6-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="bbae6-170">在**BlazorSignalRApp.Client**專案中,打開*頁面/Index.razor*檔。</span><span class="sxs-lookup"><span data-stu-id="bbae6-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="bbae6-171">將標記取代為以下代碼:</span><span class="sxs-lookup"><span data-stu-id="bbae6-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="bbae6-172">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="bbae6-172">Run the app</span></span>

1. <span data-ttu-id="bbae6-173">依工具指南操作:</span><span class="sxs-lookup"><span data-stu-id="bbae6-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbae6-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbae6-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="bbae6-175">在**解決方案資源管理器**中,選擇**BlazorSignalRApp.Server**專案。</span><span class="sxs-lookup"><span data-stu-id="bbae6-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="bbae6-176">按**Ctrl+F5**可執行應用程式而不進行調試。</span><span class="sxs-lookup"><span data-stu-id="bbae6-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="bbae6-177">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="bbae6-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bbae6-178">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbae6-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="bbae6-179">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="bbae6-179">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="bbae6-181">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="bbae6-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbae6-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbae6-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="bbae6-183">從**Debug** > 工具列中選擇 **「調試運行而不調試**」。。</span><span class="sxs-lookup"><span data-stu-id="bbae6-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="bbae6-184">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="bbae6-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bbae6-185">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbae6-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="bbae6-186">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="bbae6-186">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="bbae6-188">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="bbae6-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbae6-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bbae6-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="bbae6-190">在 **「解決方案**」側邊欄中,選擇**BlazorSignalRApp.Server**專案。</span><span class="sxs-lookup"><span data-stu-id="bbae6-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="bbae6-191">從選單中,選擇「 > **不調試即可運行啟動** **Run** 」。</span><span class="sxs-lookup"><span data-stu-id="bbae6-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="bbae6-192">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="bbae6-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bbae6-193">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbae6-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="bbae6-194">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="bbae6-194">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="bbae6-196">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="bbae6-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="bbae6-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbae6-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="bbae6-198">在命令外殼中,執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="bbae6-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="bbae6-199">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="bbae6-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bbae6-200">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbae6-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="bbae6-201">名稱和訊息立即顯示在兩個頁面上:</span><span class="sxs-lookup"><span data-stu-id="bbae6-201">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="bbae6-203">行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="bbae6-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="bbae6-204">後續步驟</span><span class="sxs-lookup"><span data-stu-id="bbae6-204">Next steps</span></span>

<span data-ttu-id="bbae6-205">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="bbae6-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbae6-206">建立BlazorWeb 群組載託管應用專案</span><span class="sxs-lookup"><span data-stu-id="bbae6-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="bbae6-207">新增SignalR用戶端函式庫</span><span class="sxs-lookup"><span data-stu-id="bbae6-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="bbae6-208">新增SignalR中心</span><span class="sxs-lookup"><span data-stu-id="bbae6-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="bbae6-209">為SignalRSignalR中心新增服務和終結點</span><span class="sxs-lookup"><span data-stu-id="bbae6-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="bbae6-210">新增用於聊天的 Razor 元件碼</span><span class="sxs-lookup"><span data-stu-id="bbae6-210">Add Razor component code for chat</span></span>

<span data-ttu-id="bbae6-211">要瞭解有關建Blazor置 應用程式的資訊Blazor, 請參考以下文件:</span><span class="sxs-lookup"><span data-stu-id="bbae6-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="bbae6-212">其他資源</span><span class="sxs-lookup"><span data-stu-id="bbae6-212">Additional resources</span></span>

* <xref:signalr/introduction>
