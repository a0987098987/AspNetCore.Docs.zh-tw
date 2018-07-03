---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077656"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="150f6-103">ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="150f6-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="150f6-104">安裝 [!INCLUDE [](~/includes/2.1-SDK.md)]。</span><span class="sxs-lookup"><span data-stu-id="150f6-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="150f6-105">建立 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="150f6-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="150f6-106">開啟命令殼層，並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="150f6-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="150f6-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="150f6-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="150f6-108">信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="150f6-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="150f6-109">Windows</span><span class="sxs-lookup"><span data-stu-id="150f6-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="150f6-110">macOS</span><span class="sxs-lookup"><span data-stu-id="150f6-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="150f6-111">Linux</span><span class="sxs-lookup"><span data-stu-id="150f6-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="150f6-112">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="150f6-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="150f6-113">瀏覽至 [http://localhost:5001](http://localhost:5001)。</span><span class="sxs-lookup"><span data-stu-id="150f6-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="150f6-114">按一下 [接受] 以接受隱私權與 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="150f6-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="150f6-115">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="150f6-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="150f6-116">開啟 *Pages/About.cshtml*，然後以下列醒目提示的標記修改頁面：</span><span class="sxs-lookup"><span data-stu-id="150f6-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="150f6-117">瀏覽到 [http://localhost:5001/About](http://localhost:5001/About) 並驗證已顯示變更。</span><span class="sxs-lookup"><span data-stu-id="150f6-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="150f6-118">安裝 [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]。</span><span class="sxs-lookup"><span data-stu-id="150f6-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="150f6-119">建立新的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="150f6-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="150f6-120">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="150f6-120">Open a command shell.</span></span> <span data-ttu-id="150f6-121">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="150f6-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="150f6-122">使用下列命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="150f6-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="150f6-123">瀏覽至 [http://localhost:5000](http://localhost:5000)。</span><span class="sxs-lookup"><span data-stu-id="150f6-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="150f6-124">開啟 *Pages/About.cshtml* 並將頁面顯示訊息修改為 "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="150f6-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="150f6-125">伺服器的時間為 @DateTime.Now“ ：</span><span class="sxs-lookup"><span data-stu-id="150f6-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="150f6-126">瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。</span><span class="sxs-lookup"><span data-stu-id="150f6-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="150f6-127">從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="150f6-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="150f6-128">為新的 ASP.NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="150f6-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="150f6-129">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="150f6-129">Open a command shell.</span></span> <span data-ttu-id="150f6-130">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="150f6-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="150f6-131">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="150f6-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="150f6-132">建立新的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="150f6-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="150f6-133">還原套件。</span><span class="sxs-lookup"><span data-stu-id="150f6-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="150f6-134">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="150f6-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="150f6-135">[dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="150f6-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="150f6-136">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="150f6-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
