---
title: "Mac、Linux 或 Windows 上的 ASP.NET Core MVC 簡介"
author: rick-anderson
description: "Mac、Linux 和 Windows 上的 ASP.NET Core MVC 與 Visual Studio Code 使用者入門"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: f439b6414d95f6edd1c2201c8aee043f1eab9b76
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="5c9b4-103">Mac、Linux 或 Windows 上的 ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="5c9b4-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="5c9b4-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5c9b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5c9b4-105">本教學課程會讓您了解使用 [Visual Studio Code](https://code.visualstudio.com) (VS Code) 建置 ASP.NET Core MVC Web 應用程式的基本知識。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="5c9b4-106">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="5c9b4-107">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="5c9b4-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="5c9b4-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="5c9b4-109">macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5c9b4-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="5c9b4-110">Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5c9b4-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="5c9b4-111">macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5c9b4-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="5c9b4-112">安裝 VS Code 和 .NET Core</span><span class="sxs-lookup"><span data-stu-id="5c9b4-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="5c9b4-113">本教學課程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="5c9b4-114">若要了解 ASP.NET Core 1.1 版，請參閱[此 PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf)。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="5c9b4-115">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="5c9b4-115">Install the following:</span></span>

* <span data-ttu-id="5c9b4-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="5c9b4-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c9b4-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="5c9b4-118">VS Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="5c9b4-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="5c9b4-119">使用 dotnet 建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="5c9b4-119">Create a web app with dotnet</span></span>

<span data-ttu-id="5c9b4-120">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="5c9b4-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="5c9b4-121">在 Visual Studio Code (VS Code) 中，開啟 *MvcMovie* 資料夾，然後選取 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="5c9b4-122">針對下列 [警告] 訊息選取 [是]：「'MvcMovie' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="5c9b4-123">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="5c9b4-123">Add them?"</span></span>
- <span data-ttu-id="5c9b4-124">針對下列 [資訊] 訊息選取 [還原]：「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code 與警告：'MvcMovie' 中遺漏了建置和偵錯的必要資產。](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="5c9b4-128">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-128">Press **Debug** (F5) to build and run the program.</span></span>

![執行中的應用程式](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="5c9b4-130">VS Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="5c9b4-131">請注意，位址列會顯示 `localhost:5000`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="5c9b4-132">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="5c9b4-133">預設範本提供您作用中的**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="5c9b4-134">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="5c9b4-135">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上角的瀏覽圖示](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="5c9b4-137">在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="5c9b4-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="5c9b4-138">Visual Studio Code 說明</span><span class="sxs-lookup"><span data-stu-id="5c9b4-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="5c9b4-139">快速入門</span><span class="sxs-lookup"><span data-stu-id="5c9b4-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="5c9b4-140">偵錯</span><span class="sxs-lookup"><span data-stu-id="5c9b4-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="5c9b4-141">整合式終端機</span><span class="sxs-lookup"><span data-stu-id="5c9b4-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="5c9b4-142">鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="5c9b4-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="5c9b4-143">Mac 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="5c9b4-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="5c9b4-144">Linux 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="5c9b4-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="5c9b4-145">Windows 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="5c9b4-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="5c9b4-146">下一步 - 新增控制器</span><span class="sxs-lookup"><span data-stu-id="5c9b4-146">Next - Add a controller</span></span>](adding-controller.md)
