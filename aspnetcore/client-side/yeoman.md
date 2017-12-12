---
title: "與 Yeoman 中 ASP.NET Core 建置專案"
author: spboyer
description: "本文逐步建置 ASP.NET Core web 應用程式使用 Yeoman macOS 上的產生器。"
keywords: "ASP.NET Core、 Yeoman、 跨平台 yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="4f6e0-104">簡介與 Yeoman 中 ASP.NET Core 建置專案</span><span class="sxs-lookup"><span data-stu-id="4f6e0-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="4f6e0-105">[Yeoman](http://yeoman.io/)是用來建立許多種類的應用程式專案 scaffolding 系統。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="4f6e0-106">Yeoman ASP.NET Core 的產生器包含各種不同的專案範本來啟動新的 web、 MVC 或主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="4f6e0-107">安裝 Node.js、 npm 及 Yeoman</span><span class="sxs-lookup"><span data-stu-id="4f6e0-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4f6e0-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="4f6e0-108">Prerequisites</span></span>

<span data-ttu-id="4f6e0-109">Node.js 及 npm 所需的 Yeoman。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="4f6e0-110">從下載[Node.js](https://nodejs.org/)。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="4f6e0-111">安裝程式包含[Node.js](https://nodejs.org/)和[npm](https://www.npmjs.com/)。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="4f6e0-112">Bower 也安裝是必要的 UI 架構，例如啟動程序。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="4f6e0-113">若要安裝 Yeoman 和 Bower，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="4f6e0-114">如果您收到錯誤`npm ERR! Please try running this command again as root/Administrator.`macOS，執行下列命令使用[sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="4f6e0-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="4f6e0-115">在命令提示字元中安裝 ASP.NET 產生器：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="4f6e0-116">如果您取得權限錯誤，請執行下的命令`sudo`上面所述。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="4f6e0-117">`–g`旗標會安裝產生器的全域，使其可用於從任何路徑。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="4f6e0-118">建立 ASP.NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="4f6e0-118">Create an ASP.NET app</span></span>

<span data-ttu-id="4f6e0-119">執行 Yeoman ASP.NET 產生器：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="4f6e0-120">產生器會顯示功能表。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-120">The generator displays a menu.</span></span> <span data-ttu-id="4f6e0-121">向下箭號**Web 應用程式基本 [沒有成員資格和授權]**專案，並點選**Enter**:</span><span class="sxs-lookup"><span data-stu-id="4f6e0-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![命令視窗： 您要建立何種應用程式？](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="4f6e0-124">選取使用者介面架構為啟動程序，然後點選**Enter**。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="4f6e0-125">使用 「**MyWebApp**」 應用程式名稱，然後點選**Enter**。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="4f6e0-126">Yeoman 會 scaffold 專案和其支援的檔案。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="4f6e0-127">表單的命令也會提供建議的後續步驟。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-127">Suggested next steps are also provided in the form of commands.</span></span>

![命令視窗： ASP.NET 應用程式的名稱是什麼？](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="4f6e0-130">[ASP.NET 產生器](https://www.npmjs.com/package/generator-aspnet)建立 ASP.NET Core 專案中，可以先載入到 Visual Studio 程式碼中，Visual Studio 中，或是從命令列執行。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="4f6e0-131">還原、 建置及執行</span><span class="sxs-lookup"><span data-stu-id="4f6e0-131">Restore, build, and run</span></span>

<span data-ttu-id="4f6e0-132">藉由變更目錄至遵循建議的命令`MyWebApp`目錄。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="4f6e0-133">然後執行`dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-133">Then run `dotnet restore`.</span></span>

![命令視窗](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="4f6e0-135">建置並執行應用程式使用`dotnet build`和`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="4f6e0-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![命令視窗](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="4f6e0-137">此時，您可以瀏覽至要測試新建立的 ASP.NET Core 應用程式顯示的 URL。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="4f6e0-138">用戶端封裝</span><span class="sxs-lookup"><span data-stu-id="4f6e0-138">Client-side packages</span></span>

<span data-ttu-id="4f6e0-139">範本所提供的前端資源 Yeoman 從產生器使用[Bower](xref:client-side/bower)用戶端封裝管理員 中，加入*bower.json*和*.bowerrc*還原使用 Bower 用戶端封裝的檔案。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="4f6e0-140">[BundlerMinifier](xref:client-side/bundling-and-minification)元件也會包含預設輕鬆串連 （結合在一起） 和縮製的 CSS、 JavaScript 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="4f6e0-141">從 Visual Studio 建置和執行</span><span class="sxs-lookup"><span data-stu-id="4f6e0-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="4f6e0-142">您可以直接將產生的 ASP.NET Core web 專案載入 Visual Studio 中，然後建置並從該處執行您的專案。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="4f6e0-143">請遵循上述指示 scaffold 使用 Yeoman 的新 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="4f6e0-144">此時，選擇**Web 應用程式**從功能表和應用程式名稱`MyWebApp`。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="4f6e0-145">開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-145">Open Visual Studio.</span></span> <span data-ttu-id="4f6e0-146">從 [檔案] 功能表中，選取 [開啟 ‣ 專案/方案]。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="4f6e0-147">在 開啟專案 對話方塊中，瀏覽至*.csproj*檔案、 選取它，然後按一下**開啟** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="4f6e0-148">在 方案總管專案看起來應該類似下面的螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![檔案和資料夾的 [方案總管] 中的新專案](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="4f6e0-150">Yeoman scaffolds MVC web 應用程式，同時使用完整伺服器和用戶端建置支援。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="4f6e0-151">伺服器端的相依性會列在下**相依性/NuGet**節點，然後用戶端中的相依性**相依性/Bower**方案總管 節點。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="4f6e0-152">載入專案時，會自動還原相依性。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-152">Dependencies are restored automatically when the project is loaded.</span></span>

![在 [方案總管] 樹狀結構檢視中的 [相依性] 節點中，Bower 資料夾是開啟列出其相依性。](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="4f6e0-154">當還原的所有相依性時，請按下**F5**執行專案。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="4f6e0-155">預設的首頁會顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-155">The default home page displays in the browser.</span></span>

![在 Microsoft Edge 中開啟的 web 應用程式](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="4f6e0-157">與還原、 建置、 裝載從命令列</span><span class="sxs-lookup"><span data-stu-id="4f6e0-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="4f6e0-158">您可以準備並裝載 web 應用程式使用.NET 核心 CLI。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="4f6e0-159">在命令提示字元中，將目前目錄變更至包含專案的資料夾 (亦即，資料夾包含*.csproj*檔案):</span><span class="sxs-lookup"><span data-stu-id="4f6e0-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="4f6e0-160">還原專案的 NuGet 封裝相依性：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="4f6e0-161">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="4f6e0-162">跨平台[Kestrel](xref:fundamentals/servers/kestrel) web 伺服器會開始接聽通訊埠 5000。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="4f6e0-163">開啟網頁瀏覽器，並導覽至`http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![在 Microsoft Edge 中開啟的 web 應用程式](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="4f6e0-165">Sub 產生器加入至專案</span><span class="sxs-lookup"><span data-stu-id="4f6e0-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="4f6e0-166">使用 Yeoman [sub 產生器](https://github.com/omnisharp/generator-aspnet)，您可以新增`nuget.config`或`web.config`建立專案之後。</span><span class="sxs-lookup"><span data-stu-id="4f6e0-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="4f6e0-167">比方說，從應該在其中建立檔案的目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="4f6e0-168">結果是名為的 NuGet 設定檔`nuget.config`具有下列內容：</span><span class="sxs-lookup"><span data-stu-id="4f6e0-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="4f6e0-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="4f6e0-169">Additional resources</span></span>

* [<span data-ttu-id="4f6e0-170">伺服器 （Kestrel 和 WebListener）</span><span class="sxs-lookup"><span data-stu-id="4f6e0-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="4f6e0-171">基礎概念</span><span class="sxs-lookup"><span data-stu-id="4f6e0-171">Fundamentals</span></span>](xref:fundamentals/index)
