---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296765"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="138db-103">ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="138db-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="138db-104">安裝 [!INCLUDE[](~/includes/2.1-SDK.md)]。</span><span class="sxs-lookup"><span data-stu-id="138db-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="138db-105">建立 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="138db-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="138db-106">開啟命令殼層，並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="138db-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="138db-108">信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="138db-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="138db-109">Windows</span><span class="sxs-lookup"><span data-stu-id="138db-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="138db-110">macOS</span><span class="sxs-lookup"><span data-stu-id="138db-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="138db-111">Linux</span><span class="sxs-lookup"><span data-stu-id="138db-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="138db-112">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="138db-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="138db-113">瀏覽至 [http://localhost:5001](http://localhost:5001)。</span><span class="sxs-lookup"><span data-stu-id="138db-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="138db-114">按一下 [接受] 以接受隱私權與 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="138db-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="138db-115">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="138db-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="138db-116">開啟 *Pages/About.cshtml*，然後以下列醒目提示的標記修改頁面：</span><span class="sxs-lookup"><span data-stu-id="138db-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="138db-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="138db-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="138db-118">瀏覽到 [http://localhost:5001/About](http://localhost:5001/About) 並驗證已顯示變更。</span><span class="sxs-lookup"><span data-stu-id="138db-118">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="138db-119">安裝 [!INCLUDE[](~/includes/net-core-sdk-download-link.md)]。</span><span class="sxs-lookup"><span data-stu-id="138db-119">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="138db-120">建立新的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="138db-120">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="138db-121">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="138db-121">Open a command shell.</span></span> <span data-ttu-id="138db-122">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="138db-122">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="138db-123">使用下列命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="138db-123">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="138db-124">瀏覽至 [http://localhost:5000](http://localhost:5000)。</span><span class="sxs-lookup"><span data-stu-id="138db-124">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="138db-125">開啟 *Pages/About.cshtml* 並將頁面顯示訊息修改為 "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="138db-125">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="138db-126">伺服器的時間為 @DateTime.Now“ ：</span><span class="sxs-lookup"><span data-stu-id="138db-126">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="138db-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="138db-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="138db-128">瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。</span><span class="sxs-lookup"><span data-stu-id="138db-128">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="138db-129">從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="138db-129">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="138db-130">為新的 ASP.NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="138db-130">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="138db-131">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="138db-131">Open a command shell.</span></span> <span data-ttu-id="138db-132">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="138db-132">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="138db-133">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="138db-133">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="138db-134">建立新的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="138db-134">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="138db-135">還原套件。</span><span class="sxs-lookup"><span data-stu-id="138db-135">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="138db-136">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="138db-136">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="138db-137">[dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="138db-137">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="138db-138">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="138db-138">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
