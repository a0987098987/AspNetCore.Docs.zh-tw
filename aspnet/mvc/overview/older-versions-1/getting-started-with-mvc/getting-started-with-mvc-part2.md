---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: 新增控制器 |Microsoft Docs
author: shanselman
description: 如果本教學課程，可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5，可提供許多增強功能，透過 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: f20889cf1c1cd9a2a69f0008d879b518c38334d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395665"
---
<a name="adding-a-controller"></a><span data-ttu-id="f811f-104">新增控制器</span><span class="sxs-lookup"><span data-stu-id="f811f-104">Adding a Controller</span></span>
====================
<span data-ttu-id="f811f-105">藉由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f811f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f811f-106">如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。</span><span class="sxs-lookup"><span data-stu-id="f811f-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="f811f-107">新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。</span><span class="sxs-lookup"><span data-stu-id="f811f-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="f811f-108">這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f811f-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f811f-109">您將建立簡單 web 應用程式，從資料庫讀取與寫入。</span><span class="sxs-lookup"><span data-stu-id="f811f-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f811f-110">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。</span><span class="sxs-lookup"><span data-stu-id="f811f-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f811f-111">MVC 代表模型、 檢視、 控制器。</span><span class="sxs-lookup"><span data-stu-id="f811f-111">MVC stands for Model, View, Controller.</span></span> <span data-ttu-id="f811f-112">MVC 是開發應用程式，使每個部分都有責任從另一個不同的模式。</span><span class="sxs-lookup"><span data-stu-id="f811f-112">MVC is a pattern for developing applications such that each part has a responsibility that is different from another.</span></span>

- <span data-ttu-id="f811f-113">您的應用程式的資料模型：</span><span class="sxs-lookup"><span data-stu-id="f811f-113">Model: The data of your application</span></span>
- <span data-ttu-id="f811f-114">檢視： 您的應用程式會使用動態產生 HTML 回應到範本檔案。</span><span class="sxs-lookup"><span data-stu-id="f811f-114">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="f811f-115">控制站： 類別，可處理傳入的 URL 要求，應用程式、 擷取模型資料，然後指定 將回應傳回給用戶端呈現的檢視範本</span><span class="sxs-lookup"><span data-stu-id="f811f-115">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response back to the client</span></span>

<span data-ttu-id="f811f-116">我們就可以在本教學課程涵蓋所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="f811f-116">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="f811f-117">以滑鼠右鍵按一下方案總管中的 控制器 資料夾，然後選取 新增控制器，讓我們來建立新的控制器。</span><span class="sxs-lookup"><span data-stu-id="f811f-117">Let's create a new controller by right-clicking the controllers folder in the solution Explorer and selecting Add Controller.</span></span>

<span data-ttu-id="f811f-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f811f-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span></span>

<span data-ttu-id="f811f-119">命名您新的控制站 」 HelloWorldController"，並按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f811f-119">Name your new controller "HelloWorldController" and click Add.</span></span>

<span data-ttu-id="f811f-120">[![新增控制器 對話方塊](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f811f-120">[![Add Controller Dialog](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span></span>

<span data-ttu-id="f811f-121">請注意，在右邊稱為 HelloWorldController.cs 您已建立新的檔案，該檔案現在會在中開啟 [方案總管] **IDE**。</span><span class="sxs-lookup"><span data-stu-id="f811f-121">Notice in the Solution Explorer on the right that a new file has been created for you called HelloWorldController.cs and that file is now opened in the **IDE**.</span></span>

<span data-ttu-id="f811f-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f811f-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span></span>

<span data-ttu-id="f811f-123">建立新公用類別 HelloWorldController 內看起來像這樣的兩個新方法。</span><span class="sxs-lookup"><span data-stu-id="f811f-123">Create two new methods that look like this inside of your new public class HelloWorldController.</span></span> <span data-ttu-id="f811f-124">我們會直接從控制器傳回 HTML 的字串，做為範例。</span><span class="sxs-lookup"><span data-stu-id="f811f-124">We'll return a string of HTML directly from our controller as an example.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

<span data-ttu-id="f811f-125">您的控制器命名為 HelloWorldController 和新方法，稱為索引。</span><span class="sxs-lookup"><span data-stu-id="f811f-125">Your Controller is named HelloWorldController and your new Method is called Index.</span></span> <span data-ttu-id="f811f-126">您再次執行應用程式，如同先前一樣 （按一下 [播放] 按鈕或按下 f5 鍵，若要這樣做）。</span><span class="sxs-lookup"><span data-stu-id="f811f-126">Run your application again, just as before (click the play button or press F5 to do this).</span></span> <span data-ttu-id="f811f-127">一旦您的瀏覽器已啟動，變更在網址列中的路徑`http://localhost:xx/HelloWorld`其中 xx 是任何數字您的電腦已選擇。</span><span class="sxs-lookup"><span data-stu-id="f811f-127">Once your browser has started up, change the path in the address bar to `http://localhost:xx/HelloWorld` where xx is whatever number your computer has chosen.</span></span> <span data-ttu-id="f811f-128">現在您的瀏覽器看起來應該像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="f811f-128">Now your browser should look like the screenshot below.</span></span> <span data-ttu-id="f811f-129">在上述我們方法我們傳回的字串傳遞至方法，稱為 [內容]。</span><span class="sxs-lookup"><span data-stu-id="f811f-129">In our method above we returned a string passed into a method called "Content."</span></span> <span data-ttu-id="f811f-130">我們只告訴系統只會傳回一些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="f811f-130">We told the system just returns some HTML, and it did!</span></span>

<span data-ttu-id="f811f-131">根據傳入 URL 不同的控制器類別 （和其中的不同動作方法），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="f811f-131">ASP.NET MVC invokes different Controller classes (and different Action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="f811f-132">使用 ASP.NET MVC 的預設對應邏輯來控制哪些程式碼會執行使用的格式如下：</span><span class="sxs-lookup"><span data-stu-id="f811f-132">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is run:</span></span>

<span data-ttu-id="f811f-133">/[Controller]/[ActionName]/[Parameters]</span><span class="sxs-lookup"><span data-stu-id="f811f-133">/[Controller]/[ActionName]/[Parameters]</span></span>

<span data-ttu-id="f811f-134">URL 的第一個部分會判斷要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="f811f-134">The first part of the URL determines the Controller class to execute.</span></span> <span data-ttu-id="f811f-135">因此 /HelloWorld 會對應至 HelloWorldController 類別。</span><span class="sxs-lookup"><span data-stu-id="f811f-135">So /HelloWorld maps to the HelloWorldController class.</span></span> <span data-ttu-id="f811f-136">URL 的第二部分會判斷要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="f811f-136">The second part of the URL determines the Action method on the class to execute.</span></span> <span data-ttu-id="f811f-137">/HelloWorld/Index 會導致要執行的 HelloWorldcontroller 類別的 index （） 方法。</span><span class="sxs-lookup"><span data-stu-id="f811f-137">So /HelloWorld/Index would cause the Index() method of the HelloWorldcontroller class to execute.</span></span> <span data-ttu-id="f811f-138">請注意，我們只有造訪上述 /HelloWorld 和索引隱含的方法。</span><span class="sxs-lookup"><span data-stu-id="f811f-138">Notice that we only had to visit /HelloWorld above and the method Index was implied.</span></span> <span data-ttu-id="f811f-139">這是因為名為"Index"的方法是將在控制器呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="f811f-139">This is because a method named "Index" is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="f811f-140">[![這是我的預設動作](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f811f-140">[![This is my default action](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span></span>

<span data-ttu-id="f811f-141">現在，讓我們來瞧`http://localhost:xx/HelloWorld/Welcome.`現在在執行我們歡迎畫面的方法，並將其傳回它的 HTML 字串。</span><span class="sxs-lookup"><span data-stu-id="f811f-141">Now, let's visit `http://localhost:xx/HelloWorld/Welcome.` Now our Welcome Method has executed and returned its HTML string.</span></span>

<span data-ttu-id="f811f-142">同樣地，/ [Controller] / [ActionName] / [Parameters] 讓控制器是 HelloWorld 和歡迎畫面在此情況下是方法。</span><span class="sxs-lookup"><span data-stu-id="f811f-142">Again, /[Controller]/[ActionName]/[Parameters] so Controller is HelloWorld and Welcome is the Method in this case.</span></span> <span data-ttu-id="f811f-143">我們還參數。</span><span class="sxs-lookup"><span data-stu-id="f811f-143">We haven't done Parameters yet.</span></span>

<span data-ttu-id="f811f-144">[![這是 歡迎使用動作方法](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f811f-144">[![This is the Welcome action method](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span></span>

<span data-ttu-id="f811f-145">讓我們的範例中稍微修改，讓我們也可以將部分資訊從 URL 中，傳遞至我們的控制器，例如像這樣: / HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4。</span><span class="sxs-lookup"><span data-stu-id="f811f-145">Let's modify our sample slightly so that we can pass some information in from the URL to our controller, for example like this: /HelloWorld/Welcome?name=Scott&amp;numtimes=4.</span></span> <span data-ttu-id="f811f-146">變更您的 歡迎使用方法，以包含兩個參數並更新其如下所示。</span><span class="sxs-lookup"><span data-stu-id="f811f-146">Change your Welcome method to include two parameters and update it like below.</span></span> <span data-ttu-id="f811f-147">請注意，我們使用 C# 選擇性參數功能，表示參數 numTimes 應該預設為 1，是否它不傳入。</span><span class="sxs-lookup"><span data-stu-id="f811f-147">Note that we've used the C# optional parameter feature to indicate that the parameter numTimes should default to 1 if it's not passed in.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

<span data-ttu-id="f811f-148">執行應用程式，並瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`任意變更名稱和 numtimes 的值。</span><span class="sxs-lookup"><span data-stu-id="f811f-148">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` changing the value of name and numtimes as you like.</span></span> <span data-ttu-id="f811f-149">系統會自動對應方法中的參數從您的查詢字串，在網址列中的具名的參數。</span><span class="sxs-lookup"><span data-stu-id="f811f-149">The system automatically mapped the named parameters from your query string in the address bar to parameters in your method.</span></span>

<span data-ttu-id="f811f-150">在這些範例中兩個控制器已執行所有的工作，並具有已直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="f811f-150">In both these examples the controller has been doing all the work, and has been returning HTML directly.</span></span> <span data-ttu-id="f811f-151">通常我們不希望我們控制站直接-傳回 HTML，因為結束程式碼很麻煩。</span><span class="sxs-lookup"><span data-stu-id="f811f-151">Ordinarily we don't want our Controllers returning HTML directly - since that ends up being very cumbersome to code.</span></span> <span data-ttu-id="f811f-152">而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="f811f-152">Instead we'll typically use a separate View template file to help generate the HTML response.</span></span> <span data-ttu-id="f811f-153">讓我們看看如何我們可以執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="f811f-153">Let's look at how we can do this.</span></span> <span data-ttu-id="f811f-154">關閉瀏覽器，並返回 IDE。</span><span class="sxs-lookup"><span data-stu-id="f811f-154">Close your browser and return to the IDE.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f811f-155">[上一頁](getting-started-with-mvc-part1.md)
> [下一頁](getting-started-with-mvc-part3.md)</span><span class="sxs-lookup"><span data-stu-id="f811f-155">[Previous](getting-started-with-mvc-part1.md)
[Next](getting-started-with-mvc-part3.md)</span></span>
