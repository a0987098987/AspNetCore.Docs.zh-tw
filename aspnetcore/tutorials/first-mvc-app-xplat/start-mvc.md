---
title: "Mac、Linux 或 Windows 上的 ASP.NET Core MVC 簡介"
author: rick-anderson
description: "Mac、Linux 和 Windows 上的 ASP.NET Core MVC 與 Visual Studio Code 使用者入門"
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: cfce91271ca21dd800fb68a14389606ce6d835f5
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="2ecfc-104">Mac、Linux 或 Windows 上的 ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="2ecfc-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="2ecfc-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ecfc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ecfc-106">本教學課程會讓您了解使用 [Visual Studio Code](https://code.visualstudio.com) (VS Code) 建置 ASP.NET Core MVC Web 應用程式的基本知識。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="2ecfc-107">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="2ecfc-108">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="2ecfc-109">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="2ecfc-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2ecfc-110">macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2ecfc-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2ecfc-111">Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2ecfc-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2ecfc-112">macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2ecfc-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="2ecfc-113">安裝 VS Code 和 .NET Core</span><span class="sxs-lookup"><span data-stu-id="2ecfc-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="2ecfc-114">本教學課程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="2ecfc-115">若要了解 ASP.NET Core 1.1 版，請參閱[此 PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf)。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="2ecfc-116">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="2ecfc-116">Install the following:</span></span>

* <span data-ttu-id="2ecfc-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="2ecfc-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ecfc-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="2ecfc-119">VS Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="2ecfc-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="2ecfc-120">使用 dotnet 建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2ecfc-120">Create a web app with dotnet</span></span>

<span data-ttu-id="2ecfc-121">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2ecfc-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="2ecfc-122">在 Visual Studio Code (VS Code) 中，開啟 *MvcMovie* 資料夾，然後選取 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="2ecfc-123">針對下列 [警告] 訊息選取 [是]：「'MvcMovie' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="2ecfc-124">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="2ecfc-124">Add them?"</span></span>
- <span data-ttu-id="2ecfc-125">選取 [還原] 至 [資訊] 訊息「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code 與警告：'MvcMovie' 中遺漏了建置和偵錯的必要資產。](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2ecfc-129">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-129">Press **Debug** (F5) to build and run the program.</span></span>

![執行中的應用程式](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="2ecfc-131">VS Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="2ecfc-132">請注意，位址列會顯示 `localhost:5000`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="2ecfc-133">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="2ecfc-134">預設範本提供您作用中的**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2ecfc-135">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2ecfc-136">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上角的瀏覽圖示](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="2ecfc-138">在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="2ecfc-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="2ecfc-139">Visual Studio Code 說明</span><span class="sxs-lookup"><span data-stu-id="2ecfc-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="2ecfc-140">快速入門</span><span class="sxs-lookup"><span data-stu-id="2ecfc-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="2ecfc-141">偵錯</span><span class="sxs-lookup"><span data-stu-id="2ecfc-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="2ecfc-142">整合式終端機</span><span class="sxs-lookup"><span data-stu-id="2ecfc-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="2ecfc-143">鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="2ecfc-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="2ecfc-144">Mac 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="2ecfc-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="2ecfc-145">Linux 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="2ecfc-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="2ecfc-146">Windows 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="2ecfc-146">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="2ecfc-147">下一步 - 新增控制器</span><span class="sxs-lookup"><span data-stu-id="2ecfc-147">Next - Add a controller</span></span>](adding-controller.md)
