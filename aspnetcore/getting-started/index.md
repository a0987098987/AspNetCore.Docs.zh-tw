---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228578"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="1cb2d-103">ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="1cb2d-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="1cb2d-104">安裝 [!INCLUDE [](~/includes/2.1-SDK.md)]。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="1cb2d-105">建立 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="1cb2d-106">開啟命令殼層，並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="1cb2d-107">信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1cb2d-108">Windows</span><span class="sxs-lookup"><span data-stu-id="1cb2d-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="1cb2d-109">上述命令會顯示以下對話方塊：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-109">The preceding command displays the following dialog:</span></span>

   ![安全性警告對話方塊](_static/cert.png)

   <span data-ttu-id="1cb2d-111">若您同意信任開發憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="1cb2d-112">macOS</span><span class="sxs-lookup"><span data-stu-id="1cb2d-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="1cb2d-113">上述命令會顯示以下訊息：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="1cb2d-114">已要求信任 HTTPS 開發憑證。若憑證尚未受到信任，我們會執行以下命令：`'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`。此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。  密碼：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="1cb2d-115">若您同意信任開發憑證，請輸入您的密碼。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="1cb2d-116">Linux</span><span class="sxs-lookup"><span data-stu-id="1cb2d-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="1cb2d-117">請參閱您 Linux 發行版的文件，來了解如何信任 HTTPS 開發憑證</span><span class="sxs-lookup"><span data-stu-id="1cb2d-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="1cb2d-118">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="1cb2d-119">瀏覽至 [http://localhost:5001](http://localhost:5001)。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="1cb2d-120">按一下 [接受] 以接受隱私權與 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="1cb2d-121">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="1cb2d-122">開啟 *Pages/About.cshtml*，然後以下列醒目提示的標記修改頁面：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="1cb2d-123">瀏覽到 [http://localhost:5001/About](http://localhost:5001/About) 並驗證已顯示變更。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="1cb2d-124">安裝 [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="1cb2d-125">建立新的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="1cb2d-126">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-126">Open a command shell.</span></span> <span data-ttu-id="1cb2d-127">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="1cb2d-128">使用下列命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="1cb2d-129">瀏覽至 [http://localhost:5000](http://localhost:5000)。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="1cb2d-130">開啟 *Pages/About.cshtml* 並將頁面顯示訊息修改為 "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="1cb2d-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="1cb2d-131">伺服器的時間為 @DateTime.Now“ ：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="1cb2d-132">瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="1cb2d-133">從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="1cb2d-134">為新的 ASP.NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="1cb2d-135">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-135">Open a command shell.</span></span> <span data-ttu-id="1cb2d-136">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="1cb2d-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="1cb2d-137">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="1cb2d-138">建立新的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="1cb2d-139">還原套件。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="1cb2d-140">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="1cb2d-141">[dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="1cb2d-142">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="1cb2d-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
