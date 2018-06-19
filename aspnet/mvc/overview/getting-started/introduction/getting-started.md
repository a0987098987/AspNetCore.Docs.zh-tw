---
uid: mvc/overview/getting-started/introduction/getting-started
title: 開始使用 ASP.NET MVC 5 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本可在此處使用 Visual Studio 2015。 新的教學課程會使用 ASP.NET Core MVC 6，提供許多 improvem...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868317"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="7f2a6-104">開始使用 ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="7f2a6-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="7f2a6-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7f2a6-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="7f2a6-106">本教學課程將告訴您建置 ASP.NET MVC 5 web 應用程式使用的基本概念[Visual Studio 2017](https://www.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="7f2a6-107">教學課程中的最後一個來源位於[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="7f2a6-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="7f2a6-108">本教學課程中所編寫的[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )， [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )與[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="7f2a6-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="7f2a6-109">您需要 Azure 帳戶，以將此應用程式部署至 Azure:</span><span class="sxs-lookup"><span data-stu-id="7f2a6-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="7f2a6-110">您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="7f2a6-111">您可以[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="7f2a6-112">快速入門</span><span class="sxs-lookup"><span data-stu-id="7f2a6-112">Getting Started</span></span>

<span data-ttu-id="7f2a6-113">開始安裝並執行[Visual Studio 2017](https://www.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="7f2a6-114">Visual Studio 是一個 IDE 中或整合式的開發環境。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="7f2a6-115">就像您使用 Microsoft Word 來寫入的文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="7f2a6-116">在 Visual Studio 中會列出沿著底部顯示各種可用的選項給您。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="7f2a6-117">也會提供另一種方式，在 IDE 中執行工作的功能表。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="7f2a6-118">(例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)</span><span class="sxs-lookup"><span data-stu-id="7f2a6-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="7f2a6-119">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="7f2a6-119">Creating Your First Application</span></span>

<span data-ttu-id="7f2a6-120">按一下**新專案**，然後選取 Visual C# 在左邊，然後**Web** ，然後選取  **ASP.NET Web 應用程式 (.NET Framework)**。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="7f2a6-121">將您的專案"MvcMovie"，再按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="7f2a6-122">在**新增 ASP.NET 專案** 對話方塊中，按一下  **MVC** ，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="7f2a6-123">Visual Studio 使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有可用的應用程式立即而不做任何事 ！</span><span class="sxs-lookup"><span data-stu-id="7f2a6-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="7f2a6-124">這是簡單"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="7f2a6-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="7f2a6-125">專案和它的啟動您的應用程式的好地方。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="7f2a6-126">按 f5 鍵啟動偵錯。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-126">Click F5 to start debugging.</span></span> <span data-ttu-id="7f2a6-127">F5 會導致 Visual Studio 啟動[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)並執行您 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="7f2a6-128">然後，visual Studio 會啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="7f2a6-129">請注意，該處會指示瀏覽器的網址列`localhost:port#`並不是類似`example.com`。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7f2a6-130">這是因為`localhost`一律會指向您自己的本機電腦，在此情況下執行您剛才建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="7f2a6-131">Visual Studio 執行時 web 專案，隨機連接埠用於 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7f2a6-132">在下面的影像中的連接埠號碼是 1234。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="7f2a6-133">當您執行應用程式時，您會看到不同的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="7f2a6-134">現成這個預設範本可讓您家用、 連絡人和相關頁面。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="7f2a6-135">不會顯示上面的影像**首頁**，**有關**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="7f2a6-136">根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示以查看這些連結。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="7f2a6-137">應用程式也提供支援註冊和登入。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="7f2a6-138">下一個步驟是變更這個應用程式的運作方式，並了解 ASP.NET MVC 的一點。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="7f2a6-139">關閉 ASP.NET MVC 應用程式，讓我們變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="7f2a6-140">如需目前的教學課程，請參閱[建議使用發行項的 MVC](../mvc-learning-sequence.md)。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="7f2a6-141">請參閱 < 在 Azure 上執行此應用程式</span><span class="sxs-lookup"><span data-stu-id="7f2a6-141">See this App Running on Azure</span></span>

<span data-ttu-id="7f2a6-142">若要查看即時 web 應用程式執行完成站台嗎？</span><span class="sxs-lookup"><span data-stu-id="7f2a6-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="7f2a6-143">您可以部署到 Azure 帳戶的完整版本的應用程式，只要按一下下列按鈕。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="7f2a6-144">您需要 Azure 帳戶，以將此方案部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="7f2a6-145">如果您沒有帳戶，您會有下列選項：</span><span class="sxs-lookup"><span data-stu-id="7f2a6-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="7f2a6-146">[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="7f2a6-147">[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。</span><span class="sxs-lookup"><span data-stu-id="7f2a6-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7f2a6-148">下一步</span><span class="sxs-lookup"><span data-stu-id="7f2a6-148">Next</span></span>](adding-a-controller.md)
