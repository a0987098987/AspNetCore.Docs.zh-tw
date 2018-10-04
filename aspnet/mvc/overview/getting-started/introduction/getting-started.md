---
uid: mvc/overview/getting-started/introduction/getting-started
title: 開始使用 ASP.NET MVC 5 |Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2cc9364b815cae0207fc59784303c6a0906f1b94
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578441"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="9dc03-102">開始使用 ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="9dc03-102">Getting started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="9dc03-103">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="9dc03-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

<span data-ttu-id="9dc03-104">本教學課程將教導您建置 ASP.NET MVC 5 web 應用程式使用的基本概念[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)。</span><span class="sxs-lookup"><span data-stu-id="9dc03-104">This tutorial teaches you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span> <span data-ttu-id="9dc03-105">本教學課程的最後一個原始程式碼位於[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)。</span><span class="sxs-lookup"><span data-stu-id="9dc03-105">The final source code for the tutorial is located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).</span></span>

<span data-ttu-id="9dc03-106">本教學課程以寫入[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )， [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )與[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="9dc03-106">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

<span data-ttu-id="9dc03-107">您需要有 Azure 帳戶才能將此應用程式部署至 Azure:</span><span class="sxs-lookup"><span data-stu-id="9dc03-107">You need an Azure account to deploy this app to Azure:</span></span>

- <span data-ttu-id="9dc03-108">您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="9dc03-108">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="9dc03-109">您可以[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="9dc03-109">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="get-started"></a><span data-ttu-id="9dc03-110">開始使用</span><span class="sxs-lookup"><span data-stu-id="9dc03-110">Get started</span></span>

<span data-ttu-id="9dc03-111">著手[安裝 Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)。</span><span class="sxs-lookup"><span data-stu-id="9dc03-111">Start by [installing Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span> <span data-ttu-id="9dc03-112">然後，開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9dc03-112">Then, open Visual Studio.</span></span>

<span data-ttu-id="9dc03-113">Visual Studio 是 IDE 或整合式的開發環境。</span><span class="sxs-lookup"><span data-stu-id="9dc03-113">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="9dc03-114">就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc03-114">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="9dc03-115">在 Visual Studio 中沒有顯示各種可用的選項，您在底端的清單。</span><span class="sxs-lookup"><span data-stu-id="9dc03-115">In Visual Studio, there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="9dc03-116">另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。</span><span class="sxs-lookup"><span data-stu-id="9dc03-116">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="9dc03-117">比方說，而不是選取**新的專案**上**起始頁**，您可以使用功能表列，並選取**檔案** > **新專案**.</span><span class="sxs-lookup"><span data-stu-id="9dc03-117">For example, instead of selecting **New Project** on the **Start page**, you can use the menu bar and select **File** > **New Project**.</span></span>

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a><span data-ttu-id="9dc03-118">建立您的第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="9dc03-118">Create your first app</span></span>

<span data-ttu-id="9dc03-119">在 **起始頁**，選取**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="9dc03-119">On the **Start page**, select **New Project**.</span></span> <span data-ttu-id="9dc03-120">在 **新的專案**對話方塊方塊中，選取**Visual C#** 左邊的類別，然後**Web**，然後選取**ASP.NET Web 應用程式 (.NET Framework)** 專案範本。</span><span class="sxs-lookup"><span data-stu-id="9dc03-120">In the **New project** dialog box, select the **Visual C#** category on the left, then **Web**, and then select the **ASP.NET Web Application (.NET Framework)** project template.</span></span> <span data-ttu-id="9dc03-121">命名您的專案"為 MvcMovie"，然後選擇 **確定**。</span><span class="sxs-lookup"><span data-stu-id="9dc03-121">Name your project "MvcMovie" and then choose **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="9dc03-122">在 **新的 ASP.NET Web 應用程式** 對話方塊中，選擇**MVC** ，然後選擇 **確定**。</span><span class="sxs-lookup"><span data-stu-id="9dc03-122">In the **New ASP.NET Web Application** dialog, choose **MVC** and then choose **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="9dc03-123">Visual Studio 使用您剛才建立的 ASP.NET MVC 專案預設範本，因此您有運作中應用程式現在無須執行任何動作 ！</span><span class="sxs-lookup"><span data-stu-id="9dc03-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="9dc03-124">這是簡單"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="9dc03-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="9dc03-125">專案和它的啟動您的應用程式的好地方。</span><span class="sxs-lookup"><span data-stu-id="9dc03-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="9dc03-126">按 **F5** 開始偵錯作業。</span><span class="sxs-lookup"><span data-stu-id="9dc03-126">Press **F5** to start debugging.</span></span> <span data-ttu-id="9dc03-127">當您按下**F5**，Visual Studio 會啟動[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)並執行您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc03-127">When you press **F5**, Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your web app.</span></span> <span data-ttu-id="9dc03-128">Visual Studio 接著會啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="9dc03-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="9dc03-129">通知，指出瀏覽器的網址列`localhost:port#`而不是類似`example.com`。</span><span class="sxs-lookup"><span data-stu-id="9dc03-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9dc03-130">這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc03-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="9dc03-131">Visual Studio 執行時 web 專案，會將隨機的連接埠用於 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="9dc03-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="9dc03-132">在下面的影像中的連接埠號碼是 1234。</span><span class="sxs-lookup"><span data-stu-id="9dc03-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="9dc03-133">當您執行應用程式時，您會看到不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="9dc03-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="9dc03-134">現成這個預設範本可讓您`Home`， `Contact`，和`About`頁面。</span><span class="sxs-lookup"><span data-stu-id="9dc03-134">Right out of the box this default template gives you `Home`, `Contact`, and `About` pages.</span></span> <span data-ttu-id="9dc03-135">上述映像不會顯示**首頁**，**有關**，並**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="9dc03-135">The image above doesn't show the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="9dc03-136">根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示，以查看這些連結。</span><span class="sxs-lookup"><span data-stu-id="9dc03-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="9dc03-137">應用程式也會提供註冊和登入的支援。</span><span class="sxs-lookup"><span data-stu-id="9dc03-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="9dc03-138">下一個步驟是要變更此應用程式的運作方式有點了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="9dc03-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="9dc03-139">關閉 ASP.NET MVC 應用程式，並讓我們變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="9dc03-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="9dc03-140">如需目前的教學課程的清單，請參閱 < [MVC 建議文章](../mvc-learning-sequence.md)。</span><span class="sxs-lookup"><span data-stu-id="9dc03-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="9dc03-141">請參閱在 Azure 上執行此應用程式</span><span class="sxs-lookup"><span data-stu-id="9dc03-141">See this app running on Azure</span></span>

<span data-ttu-id="9dc03-142">若要查看為即時 web 應用程式正在執行的完成的網站嗎？</span><span class="sxs-lookup"><span data-stu-id="9dc03-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="9dc03-143">您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="9dc03-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="9dc03-144">您需要有 Azure 帳戶才能將此解決方案部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="9dc03-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="9dc03-145">如果您還沒有帳戶，請使用下列選項之一來建立一個：</span><span class="sxs-lookup"><span data-stu-id="9dc03-145">If you don't already have an account, use one of the following options to create one:</span></span>

- <span data-ttu-id="9dc03-146">[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="9dc03-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="9dc03-147">[啟用 Visual Studio 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers)-您的 Visual Studio 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="9dc03-147">[Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9dc03-148">下一步</span><span class="sxs-lookup"><span data-stu-id="9dc03-148">Next</span></span>](adding-a-controller.md)
