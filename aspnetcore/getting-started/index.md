---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4a5a0cc5a5dab2171ab8ef43818185a4ee91af0e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912562"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="b24f4-103">教學課程：ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="b24f4-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="b24f4-104">本教學課程示範如何使用 .NET Core 命令列介面來建立 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b24f4-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="b24f4-105">您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b24f4-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b24f4-106">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="b24f4-106">Create a web app project.</span></span>
> * <span data-ttu-id="b24f4-107">啟用本機 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="b24f4-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="b24f4-108">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b24f4-108">Run the app.</span></span>
> * <span data-ttu-id="b24f4-109">編輯 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="b24f4-109">Edit a Razor page.</span></span>

<span data-ttu-id="b24f4-110">最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b24f4-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web 應用程式首頁](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="b24f4-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="b24f4-112">Prerequisites</span></span>

* <span data-ttu-id="b24f4-113">安裝 [!INCLUDE [](~/includes/2.1-SDK.md)]。</span><span class="sxs-lookup"><span data-stu-id="b24f4-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="b24f4-114">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="b24f4-114">Create a web app project</span></span>

* <span data-ttu-id="b24f4-115">開啟命令殼層，並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b24f4-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="b24f4-116">啟用本機 HTTPS</span><span class="sxs-lookup"><span data-stu-id="b24f4-116">Enable local HTTPS</span></span>

* <span data-ttu-id="b24f4-117">信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="b24f4-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b24f4-118">Windows</span><span class="sxs-lookup"><span data-stu-id="b24f4-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="b24f4-119">上述命令會顯示以下對話方塊：</span><span class="sxs-lookup"><span data-stu-id="b24f4-119">The preceding command displays the following dialog:</span></span>

  ![安全性警告對話方塊](_static/cert.png)

  <span data-ttu-id="b24f4-121">若您同意信任開發憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="b24f4-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b24f4-122">macOS</span><span class="sxs-lookup"><span data-stu-id="b24f4-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="b24f4-123">上述命令會顯示以下訊息：</span><span class="sxs-lookup"><span data-stu-id="b24f4-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="b24f4-124">已要求信任 HTTPS 開發憑證。若憑證尚未受到信任，我們會執行下列命令：\*`'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`。</span><span class="sxs-lookup"><span data-stu-id="b24f4-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="b24f4-125">\*此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。</span><span class="sxs-lookup"><span data-stu-id="b24f4-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="b24f4-126">密碼：\*</span><span class="sxs-lookup"><span data-stu-id="b24f4-126">Password:\*</span></span>

  <span data-ttu-id="b24f4-127">若您同意信任開發憑證，請輸入您的密碼。</span><span class="sxs-lookup"><span data-stu-id="b24f4-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b24f4-128">Linux</span><span class="sxs-lookup"><span data-stu-id="b24f4-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="b24f4-129">請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="b24f4-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="b24f4-130">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b24f4-130">Run the app</span></span>

* <span data-ttu-id="b24f4-131">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b24f4-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="b24f4-132">瀏覽至 [https://localhost:5001](https://localhost:5001)。</span><span class="sxs-lookup"><span data-stu-id="b24f4-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="b24f4-133">按一下 [接受] 以接受隱私權與 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="b24f4-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="b24f4-134">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="b24f4-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="b24f4-135">編輯 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="b24f4-135">Edit a Razor page</span></span>

* <span data-ttu-id="b24f4-136">開啟 *Pages/About.cshtml*，然後以下列醒目提示的標記修改頁面：</span><span class="sxs-lookup"><span data-stu-id="b24f4-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="b24f4-137">瀏覽到 [https://localhost:5001/About](https://localhost:5001/About) 並驗證已顯示變更。</span><span class="sxs-lookup"><span data-stu-id="b24f4-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b24f4-138">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b24f4-138">Next steps</span></span>

<span data-ttu-id="b24f4-139">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b24f4-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b24f4-140">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="b24f4-140">Create a web app project.</span></span>
> * <span data-ttu-id="b24f4-141">啟用本機 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="b24f4-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="b24f4-142">執行專案。</span><span class="sxs-lookup"><span data-stu-id="b24f4-142">Run the project.</span></span>
> * <span data-ttu-id="b24f4-143">進行變更。</span><span class="sxs-lookup"><span data-stu-id="b24f4-143">Make a change.</span></span>

<span data-ttu-id="b24f4-144">若要深入了解 ASP.NET Core，請參閱簡介：</span><span class="sxs-lookup"><span data-stu-id="b24f4-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
