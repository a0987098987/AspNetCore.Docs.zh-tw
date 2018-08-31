---
title: macOS、Linux 或 Windows 上的 ASP.NET Core MVC 簡介
author: rick-anderson
description: 了解 macOS、Linux 和 Windows 上的 ASP.NET Core MVC 與 Visual Studio Code 使用者入門
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "36275271"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="05c65-103">macOS、Linux 或 Windows 上的 ASP.NET Core MVC 簡介</span><span class="sxs-lookup"><span data-stu-id="05c65-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="05c65-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="05c65-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="05c65-105">本教學課程會讓您了解使用 [Visual Studio Code](https://code.visualstudio.com) (VS Code) 建置 ASP.NET Core MVC Web 應用程式的基本知識。</span><span class="sxs-lookup"><span data-stu-id="05c65-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="05c65-106">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="05c65-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="05c65-107">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="05c65-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="05c65-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="05c65-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="05c65-109">macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="05c65-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="05c65-110">Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="05c65-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="05c65-111">macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="05c65-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="05c65-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="05c65-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="05c65-113">使用 dotnet 建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="05c65-113">Create a web app with dotnet</span></span>

<span data-ttu-id="05c65-114">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="05c65-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="05c65-115">在 Visual Studio Code (VS Code) 中，開啟 *MvcMovie* 資料夾，然後選取 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="05c65-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="05c65-116">針對下列 [警告] 訊息選取 [是]：「'MvcMovie' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="05c65-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="05c65-117">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="05c65-117">Add them?"</span></span>
- <span data-ttu-id="05c65-118">針對下列 [資訊] 訊息選取 [還原]：「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="05c65-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code 與警告：'MvcMovie' 中遺漏了建置和偵錯的必要資產。](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="05c65-122">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="05c65-122">Press **Debug** (F5) to build and run the program.</span></span>

![執行中的應用程式](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="05c65-124">VS Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="05c65-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="05c65-125">請注意，位址列會顯示 `localhost:5000`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="05c65-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="05c65-126">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="05c65-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="05c65-127">預設範本提供您作用中的**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="05c65-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="05c65-128">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="05c65-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="05c65-129">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="05c65-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上角的瀏覽圖示](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="05c65-131">在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="05c65-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="05c65-132">Visual Studio Code 說明</span><span class="sxs-lookup"><span data-stu-id="05c65-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="05c65-133">快速入門</span><span class="sxs-lookup"><span data-stu-id="05c65-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="05c65-134">偵錯</span><span class="sxs-lookup"><span data-stu-id="05c65-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="05c65-135">整合式終端機</span><span class="sxs-lookup"><span data-stu-id="05c65-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="05c65-136">鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="05c65-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="05c65-137">macOS 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="05c65-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="05c65-138">Linux 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="05c65-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="05c65-139">Windows 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="05c65-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="05c65-140">下一步 - 新增控制器</span><span class="sxs-lookup"><span data-stu-id="05c65-140">Next - Add a controller</span></span>](adding-controller.md)
