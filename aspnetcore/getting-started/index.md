---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 此教學課程時間不長，會使用 ASP.NET Core 建立及執行基本的 Hello World 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 4f7e67e1e422afe3f7e2970e0c40380f065390ac
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928323"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="8db1f-103">教學課程：ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="8db1f-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="8db1f-104">本教學課程說明如何使用 .NET Core CLI 來建立及執行 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8db1f-104">This tutorial shows how to use the .NET Core CLI to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="8db1f-105">您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="8db1f-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8db1f-106">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8db1f-106">Create a web app project.</span></span>
> * <span data-ttu-id="8db1f-107">信任開發憑證。</span><span class="sxs-lookup"><span data-stu-id="8db1f-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="8db1f-108">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8db1f-108">Run the app.</span></span>
> * <span data-ttu-id="8db1f-109">編輯 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="8db1f-109">Edit a Razor page.</span></span>

<span data-ttu-id="8db1f-110">最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8db1f-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web 應用程式首頁](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="8db1f-112">必要條件：</span><span class="sxs-lookup"><span data-stu-id="8db1f-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="8db1f-113">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="8db1f-113">Create a web app project</span></span>

<span data-ttu-id="8db1f-114">開啟命令殼層，並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="8db1f-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="8db1f-115">上述命令：</span><span class="sxs-lookup"><span data-stu-id="8db1f-115">The preceding command:</span></span>

* <span data-ttu-id="8db1f-116">建立新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8db1f-116">Creates a new web app.</span></span>  
* <span data-ttu-id="8db1f-117">`-o aspnetcoreapp` 參數會建立名為*aspnetcoreapp*的目錄，其中包含應用程式的來源檔案。</span><span class="sxs-lookup"><span data-stu-id="8db1f-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="8db1f-118">信任開發憑證</span><span class="sxs-lookup"><span data-stu-id="8db1f-118">Trust the development certificate</span></span>

<span data-ttu-id="8db1f-119">信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="8db1f-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="8db1f-120">Windows</span><span class="sxs-lookup"><span data-stu-id="8db1f-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="8db1f-121">上述命令會顯示以下對話方塊：</span><span class="sxs-lookup"><span data-stu-id="8db1f-121">The preceding command displays the following dialog:</span></span>

![安全性警告對話方塊](~/getting-started/_static/cert.png)

<span data-ttu-id="8db1f-123">若您同意信任開發憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="8db1f-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="8db1f-124">macOS</span><span class="sxs-lookup"><span data-stu-id="8db1f-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="8db1f-125">上述命令會顯示以下訊息：</span><span class="sxs-lookup"><span data-stu-id="8db1f-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="8db1f-126">*已要求信任 HTTPS 開發憑證。如果憑證尚未受到信任，我們會執行下列命令：* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="8db1f-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted, we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="8db1f-127">此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。</span><span class="sxs-lookup"><span data-stu-id="8db1f-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="8db1f-128">若您同意信任開發憑證，請輸入您的密碼。</span><span class="sxs-lookup"><span data-stu-id="8db1f-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="8db1f-129">Linux</span><span class="sxs-lookup"><span data-stu-id="8db1f-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="8db1f-130">請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="8db1f-130">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="8db1f-131">如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="8db1f-131">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8db1f-132">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="8db1f-132">Run the app</span></span>

<span data-ttu-id="8db1f-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8db1f-133">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="8db1f-134">命令殼層指出應用程式已啟動之後，瀏覽到 [https://localhost:5001](https://localhost:5001)。</span><span class="sxs-lookup"><span data-stu-id="8db1f-134">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="8db1f-135">編輯 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="8db1f-135">Edit a Razor page</span></span>

<span data-ttu-id="8db1f-136">開啟*Pages/Index. cshtml* ，並以下列反白顯示的標記修改並儲存頁面：</span><span class="sxs-lookup"><span data-stu-id="8db1f-136">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="8db1f-137">流覽至[https://localhost:5001](https://localhost:5001)、重新整理頁面，並確認已顯示變更。</span><span class="sxs-lookup"><span data-stu-id="8db1f-137">Browse to [https://localhost:5001](https://localhost:5001), refresh the page, and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8db1f-138">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8db1f-138">Next steps</span></span>

<span data-ttu-id="8db1f-139">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="8db1f-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8db1f-140">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8db1f-140">Create a web app project.</span></span>
> * <span data-ttu-id="8db1f-141">信任開發憑證。</span><span class="sxs-lookup"><span data-stu-id="8db1f-141">Trust the development certificate.</span></span>
> * <span data-ttu-id="8db1f-142">執行專案。</span><span class="sxs-lookup"><span data-stu-id="8db1f-142">Run the project.</span></span>
> * <span data-ttu-id="8db1f-143">進行變更。</span><span class="sxs-lookup"><span data-stu-id="8db1f-143">Make a change.</span></span>

<span data-ttu-id="8db1f-144">若要深入了解 ASP.NET Core，請參閱簡介中的建議學習路徑：</span><span class="sxs-lookup"><span data-stu-id="8db1f-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
