---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: 加入控制器 |Microsoft 文件
author: shanselman
description: 本教學課程是否可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5 提供許多改良 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869019"
---
<a name="adding-a-controller"></a><span data-ttu-id="f3256-104">新增控制器</span><span class="sxs-lookup"><span data-stu-id="f3256-104">Adding a Controller</span></span>
====================
<span data-ttu-id="f3256-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f3256-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f3256-106">如果本教學課程中可用的更新的版本[這裡](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。</span><span class="sxs-lookup"><span data-stu-id="f3256-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="f3256-107">新的教學課程會使用 ASP.NET MVC 5，本教學課程提供許多改良。</span><span class="sxs-lookup"><span data-stu-id="f3256-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="f3256-108">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="f3256-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f3256-109">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f3256-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f3256-110">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="f3256-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f3256-111">MVC 代表模型、 檢視、 控制站。</span><span class="sxs-lookup"><span data-stu-id="f3256-111">MVC stands for Model, View, Controller.</span></span> <span data-ttu-id="f3256-112">MVC 是用於開發應用程式，使每個部分都有不同於另一個有責任的模式。</span><span class="sxs-lookup"><span data-stu-id="f3256-112">MVC is a pattern for developing applications such that each part has a responsibility that is different from another.</span></span>

- <span data-ttu-id="f3256-113">您的應用程式的資料模型：</span><span class="sxs-lookup"><span data-stu-id="f3256-113">Model: The data of your application</span></span>
- <span data-ttu-id="f3256-114">檢視： 您的應用程式將用來動態產生 HTML 回應範本檔案。</span><span class="sxs-lookup"><span data-stu-id="f3256-114">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="f3256-115">控制站： 類別，處理內送的 URL 要求，應用程式，可擷取模型資料，然後指定 將回應傳回給用戶端呈現的檢視範本</span><span class="sxs-lookup"><span data-stu-id="f3256-115">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response back to the client</span></span>

<span data-ttu-id="f3256-116">我們將會在本教學課程涵蓋所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="f3256-116">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="f3256-117">以滑鼠右鍵按一下方案總管中的 控制器 資料夾，然後選取 加入控制器，讓我們來建立新的控制站。</span><span class="sxs-lookup"><span data-stu-id="f3256-117">Let's create a new controller by right-clicking the controllers folder in the solution Explorer and selecting Add Controller.</span></span>

<span data-ttu-id="f3256-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3256-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span></span>

<span data-ttu-id="f3256-119">命名您的新控制器"HelloWorldController"，並按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f3256-119">Name your new controller "HelloWorldController" and click Add.</span></span>

<span data-ttu-id="f3256-120">[![加入控制器 對話方塊](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f3256-120">[![Add Controller Dialog](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span></span>

<span data-ttu-id="f3256-121">請注意，在 [方案總管]，右邊已為您呼叫 HelloWorldController.cs 建立新的檔案，該檔案現在會在中開啟**IDE**。</span><span class="sxs-lookup"><span data-stu-id="f3256-121">Notice in the Solution Explorer on the right that a new file has been created for you called HelloWorldController.cs and that file is now opened in the **IDE**.</span></span>

<span data-ttu-id="f3256-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f3256-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span></span>

<span data-ttu-id="f3256-123">建立新公用類別 HelloWorldController 內看起來像這樣的兩個新方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-123">Create two new methods that look like this inside of your new public class HelloWorldController.</span></span> <span data-ttu-id="f3256-124">做為範例，我們將直接從 controller 傳回 HTML 的字串。</span><span class="sxs-lookup"><span data-stu-id="f3256-124">We'll return a string of HTML directly from our controller as an example.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

<span data-ttu-id="f3256-125">將控制器命名為 HelloWorldController，而且您的新方法呼叫索引。</span><span class="sxs-lookup"><span data-stu-id="f3256-125">Your Controller is named HelloWorldController and your new Method is called Index.</span></span> <span data-ttu-id="f3256-126">您再次執行應用程式，只要如常 （按一下 [開始] 按鈕或按 f5 鍵，若要這樣做）。</span><span class="sxs-lookup"><span data-stu-id="f3256-126">Run your application again, just as before (click the play button or press F5 to do this).</span></span> <span data-ttu-id="f3256-127">一旦您的瀏覽器已啟動，變更 網址列中的路徑`http://localhost:xx/HelloWorld`其中 xx 是任意數字您的電腦已選擇。</span><span class="sxs-lookup"><span data-stu-id="f3256-127">Once your browser has started up, change the path in the address bar to `http://localhost:xx/HelloWorld` where xx is whatever number your computer has chosen.</span></span> <span data-ttu-id="f3256-128">現在您的瀏覽器應該看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="f3256-128">Now your browser should look like the screenshot below.</span></span> <span data-ttu-id="f3256-129">在我們上述方法，我們傳回字串傳遞給方法，稱為 [內容]。</span><span class="sxs-lookup"><span data-stu-id="f3256-129">In our method above we returned a string passed into a method called "Content."</span></span> <span data-ttu-id="f3256-130">我們會告訴系統只會傳回某些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="f3256-130">We told the system just returns some HTML, and it did!</span></span>

<span data-ttu-id="f3256-131">根據傳入的 URL 不同控制器類別 （和其中的不同動作方法），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="f3256-131">ASP.NET MVC invokes different Controller classes (and different Action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="f3256-132">使用 ASP.NET MVC 的預設對應邏輯來控制哪些程式碼會執行使用的格式如下：</span><span class="sxs-lookup"><span data-stu-id="f3256-132">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is run:</span></span>

<span data-ttu-id="f3256-133">/[Controller]/[ActionName]/[Parameters]</span><span class="sxs-lookup"><span data-stu-id="f3256-133">/[Controller]/[ActionName]/[Parameters]</span></span>

<span data-ttu-id="f3256-134">URL 的第一個部分會決定要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="f3256-134">The first part of the URL determines the Controller class to execute.</span></span> <span data-ttu-id="f3256-135">因此 /HelloWorld 會對應至 HelloWorldController 類別。</span><span class="sxs-lookup"><span data-stu-id="f3256-135">So /HelloWorld maps to the HelloWorldController class.</span></span> <span data-ttu-id="f3256-136">URL 的第二個部分會決定要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-136">The second part of the URL determines the Action method on the class to execute.</span></span> <span data-ttu-id="f3256-137">/HelloWorld/Index 會導致要執行的 HelloWorldcontroller 類別的 index （） 方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-137">So /HelloWorld/Index would cause the Index() method of the HelloWorldcontroller class to execute.</span></span> <span data-ttu-id="f3256-138">請注意，我們只需要瀏覽 /HelloWorld 上述和索引所隱含的方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-138">Notice that we only had to visit /HelloWorld above and the method Index was implied.</span></span> <span data-ttu-id="f3256-139">這是因為名為"Index"的方法是將會在控制站呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-139">This is because a method named "Index" is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="f3256-140">[![這是我的預設動作](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f3256-140">[![This is my default action](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span></span>

<span data-ttu-id="f3256-141">現在，讓我們來瀏覽`http://localhost:xx/HelloWorld/Welcome.`現在我們  褖畫惎方法在執行，並傳回其 HTML 字串。</span><span class="sxs-lookup"><span data-stu-id="f3256-141">Now, let's visit `http://localhost:xx/HelloWorld/Welcome.` Now our Welcome Method has executed and returned its HTML string.</span></span>

<span data-ttu-id="f3256-142">同樣地，/ [控制器] / [ActionName] / [參數] 因此控制器 HelloWorld 而且歡迎畫面在此情況下是方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-142">Again, /[Controller]/[ActionName]/[Parameters] so Controller is HelloWorld and Welcome is the Method in this case.</span></span> <span data-ttu-id="f3256-143">我們還參數。</span><span class="sxs-lookup"><span data-stu-id="f3256-143">We haven't done Parameters yet.</span></span>

<span data-ttu-id="f3256-144">[![這是該  褖畫惎動作方法](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f3256-144">[![This is the Welcome action method](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span></span>

<span data-ttu-id="f3256-145">讓我們的範例稍微修改，讓我們可以從 URL 傳遞中的某些資訊至我們的控制站，例如就像這樣: / HelloWorld/歡迎畫面？ name = Scott&amp;numtimes = 4。</span><span class="sxs-lookup"><span data-stu-id="f3256-145">Let's modify our sample slightly so that we can pass some information in from the URL to our controller, for example like this: /HelloWorld/Welcome?name=Scott&amp;numtimes=4.</span></span> <span data-ttu-id="f3256-146">變更您  褖畫惎方法，將包含兩個參數和它類似下面的的更新。</span><span class="sxs-lookup"><span data-stu-id="f3256-146">Change your Welcome method to include two parameters and update it like below.</span></span> <span data-ttu-id="f3256-147">請注意，我們使用 C# 選擇性參數功能來指示參數 numTimes 應該預設為 1，是否它不會傳遞中。</span><span class="sxs-lookup"><span data-stu-id="f3256-147">Note that we've used the C# optional parameter feature to indicate that the parameter numTimes should default to 1 if it's not passed in.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

<span data-ttu-id="f3256-148">執行您的應用程式，請瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`任意變更名稱和 numtimes 的值。</span><span class="sxs-lookup"><span data-stu-id="f3256-148">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` changing the value of name and numtimes as you like.</span></span> <span data-ttu-id="f3256-149">系統會自動對應具名的參數從您在網址列中的查詢字串參數，在您的方法。</span><span class="sxs-lookup"><span data-stu-id="f3256-149">The system automatically mapped the named parameters from your query string in the address bar to parameters in your method.</span></span>

<span data-ttu-id="f3256-150">在這些範例中這兩個控制器已執行的所有工作，並具有已傳回 HTML 直接。</span><span class="sxs-lookup"><span data-stu-id="f3256-150">In both these examples the controller has been doing all the work, and has been returning HTML directly.</span></span> <span data-ttu-id="f3256-151">通常，我們不希望我們控制站直接-傳回 HTML，因為，最後會在程式碼很麻煩。</span><span class="sxs-lookup"><span data-stu-id="f3256-151">Ordinarily we don't want our Controllers returning HTML directly - since that ends up being very cumbersome to code.</span></span> <span data-ttu-id="f3256-152">改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="f3256-152">Instead we'll typically use a separate View template file to help generate the HTML response.</span></span> <span data-ttu-id="f3256-153">讓我們看看如何我們可以執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="f3256-153">Let's look at how we can do this.</span></span> <span data-ttu-id="f3256-154">關閉瀏覽器，並返回 IDE。</span><span class="sxs-lookup"><span data-stu-id="f3256-154">Close your browser and return to the IDE.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f3256-155">[上一頁](getting-started-with-mvc-part1.md)
> [下一頁](getting-started-with-mvc-part3.md)</span><span class="sxs-lookup"><span data-stu-id="f3256-155">[Previous](getting-started-with-mvc-part1.md)
[Next](getting-started-with-mvc-part3.md)</span></span>
