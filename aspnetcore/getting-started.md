---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="0dc61-103">ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="0dc61-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="0dc61-104">安裝 [!INCLUDE[](~/includes/net-core-sdk-download-link.md)]。</span><span class="sxs-lookup"><span data-stu-id="0dc61-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="0dc61-105">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="0dc61-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="0dc61-106">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="0dc61-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0dc61-107">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="0dc61-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="0dc61-108">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="0dc61-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="0dc61-109">使用下列命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="0dc61-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="0dc61-110">瀏覽至 [http://localhost:5000](http://localhost:5000)。</span><span class="sxs-lookup"><span data-stu-id="0dc61-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="0dc61-111">開啟 *Pages/About.cshtml* 並將頁面顯示訊息修改為 "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="0dc61-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="0dc61-112">伺服器的時間為 @DateTime.Now“ ：</span><span class="sxs-lookup"><span data-stu-id="0dc61-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="0dc61-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="0dc61-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="0dc61-114">瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。</span><span class="sxs-lookup"><span data-stu-id="0dc61-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="0dc61-115">從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="0dc61-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="0dc61-116">為新的 .NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="0dc61-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="0dc61-117">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="0dc61-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0dc61-118">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="0dc61-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="0dc61-119">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="0dc61-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="0dc61-120">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="0dc61-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="0dc61-121">還原套件。</span><span class="sxs-lookup"><span data-stu-id="0dc61-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="0dc61-122">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0dc61-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="0dc61-123">[dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="0dc61-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="0dc61-124">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="0dc61-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end