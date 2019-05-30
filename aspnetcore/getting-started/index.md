---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 此教學課程時間不長，會使用 ASP.NET Core 建立及執行基本的 Hello World 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 5/15/2019
uid: getting-started
ms.openlocfilehash: 9227dcfbc84376d9d73bc6fc0dd76085779acae1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610318"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="4cec2-103">教學課程：ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="4cec2-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="4cec2-104">此教學課程示範如何使用.NET Core 命令列介面建立及執行 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4cec2-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="4cec2-105">您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="4cec2-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cec2-106">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="4cec2-106">Create a web app project.</span></span>
> * <span data-ttu-id="4cec2-107">信任開發憑證。</span><span class="sxs-lookup"><span data-stu-id="4cec2-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="4cec2-108">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4cec2-108">Run the app.</span></span>
> * <span data-ttu-id="4cec2-109">編輯 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="4cec2-109">Edit a Razor page.</span></span>

<span data-ttu-id="4cec2-110">最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4cec2-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web 應用程式首頁](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="4cec2-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="4cec2-112">Prerequisites</span></span>

* [<span data-ttu-id="4cec2-113">.NET Core 2.2 SDK</span><span class="sxs-lookup"><span data-stu-id="4cec2-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="4cec2-114">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="4cec2-114">Create a web app project</span></span>

<span data-ttu-id="4cec2-115">開啟命令殼層，並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="4cec2-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="4cec2-116">信任開發憑證</span><span class="sxs-lookup"><span data-stu-id="4cec2-116">Trust the development certificate</span></span>

<span data-ttu-id="4cec2-117">信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="4cec2-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4cec2-118">Windows</span><span class="sxs-lookup"><span data-stu-id="4cec2-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="4cec2-119">上述命令會顯示以下對話方塊：</span><span class="sxs-lookup"><span data-stu-id="4cec2-119">The preceding command displays the following dialog:</span></span>

![安全性警告對話方塊](~/getting-started/_static/cert.png)

<span data-ttu-id="4cec2-121">若您同意信任開發憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="4cec2-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="4cec2-122">macOS</span><span class="sxs-lookup"><span data-stu-id="4cec2-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="4cec2-123">上述命令會顯示以下訊息：</span><span class="sxs-lookup"><span data-stu-id="4cec2-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="4cec2-124">*已要求信任 HTTPS 開發憑證。若憑證尚未受到信任，我們會執行下列命令：* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="4cec2-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="4cec2-125">此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。</span><span class="sxs-lookup"><span data-stu-id="4cec2-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="4cec2-126">若您同意信任開發憑證，請輸入您的密碼。</span><span class="sxs-lookup"><span data-stu-id="4cec2-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4cec2-127">Linux</span><span class="sxs-lookup"><span data-stu-id="4cec2-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="4cec2-128">若是適用於 Linux 的 Windows 子系統，請參閱[信任來自適用於 Linux 的 Windows 子系統的 HTTPS 憑證](xref:security/enforcing-ssl#wsl)。</span><span class="sxs-lookup"><span data-stu-id="4cec2-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="4cec2-129">請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="4cec2-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="4cec2-130">如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="4cec2-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4cec2-131">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="4cec2-131">Run the app</span></span>

<span data-ttu-id="4cec2-132">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4cec2-132">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="4cec2-133">命令殼層指出應用程式已啟動之後，瀏覽到 [https://localhost:5001](https://localhost:5001)。</span><span class="sxs-lookup"><span data-stu-id="4cec2-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="4cec2-134">按一下 [接受] 以接受隱私權與 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="4cec2-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="4cec2-135">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="4cec2-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="4cec2-136">編輯 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="4cec2-136">Edit a Razor page</span></span>

<span data-ttu-id="4cec2-137">開啟 *Pages/Index.cshtml*，然後以下列醒目提示的標記修改頁面：</span><span class="sxs-lookup"><span data-stu-id="4cec2-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="4cec2-138">瀏覽到 [https://localhost:5001](https://localhost:5001) 並驗證已顯示變更。</span><span class="sxs-lookup"><span data-stu-id="4cec2-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cec2-139">後續步驟</span><span class="sxs-lookup"><span data-stu-id="4cec2-139">Next steps</span></span>

<span data-ttu-id="4cec2-140">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="4cec2-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cec2-141">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="4cec2-141">Create a web app project.</span></span>
> * <span data-ttu-id="4cec2-142">信任開發憑證。</span><span class="sxs-lookup"><span data-stu-id="4cec2-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="4cec2-143">執行專案。</span><span class="sxs-lookup"><span data-stu-id="4cec2-143">Run the project.</span></span>
> * <span data-ttu-id="4cec2-144">進行變更。</span><span class="sxs-lookup"><span data-stu-id="4cec2-144">Make a change.</span></span>

<span data-ttu-id="4cec2-145">若要深入了解 ASP.NET Core，請參閱簡介中的建議學習路徑：</span><span class="sxs-lookup"><span data-stu-id="4cec2-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
