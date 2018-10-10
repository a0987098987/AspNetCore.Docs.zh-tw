---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET MVC 4 簡介 |Microsoft Docs
author: Rick-Anderson
description: 如果本教學課程，可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5，可提供許多增強功能，透過 t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: ea3d1517192ded0e5372c49897bb1fec33324b6f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912393"
---
<a name="intro-to-aspnet-mvc-4"></a><span data-ttu-id="96f87-104">ASP.NET MVC 4 簡介</span><span class="sxs-lookup"><span data-stu-id="96f87-104">Intro to ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="96f87-105">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="96f87-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="96f87-106">如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)。</span><span class="sxs-lookup"><span data-stu-id="96f87-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="96f87-107">新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。</span><span class="sxs-lookup"><span data-stu-id="96f87-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
> <span data-ttu-id="96f87-108">本教學課程將教導您建置使用 Microsoft ASP.NET MVC 4 Web 應用程式的基本概念[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="96f87-108">This tutorial will teach you the basics of building an ASP.NET MVC 4 Web application using Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) or Visual Web Developer 2010 Express Service Pack 1.</span></span> <span data-ttu-id="96f87-109">Visual Studio 2012，建議您不需要安裝任何項目才能完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="96f87-109">Visual Studio 2012 is recommended, you won't need to install anything to complete the tutorial.</span></span> <span data-ttu-id="96f87-110">如果您使用 Visual Studio 2010，您必須安裝下列元件。</span><span class="sxs-lookup"><span data-stu-id="96f87-110">If you are using Visual Studio 2010 you must install the components below.</span></span> <span data-ttu-id="96f87-111">您可以安裝所有人都依序按一下下列連結：</span><span class="sxs-lookup"><span data-stu-id="96f87-111">You can install all of them by clicking the following links:</span></span>
>
> - [<span data-ttu-id="96f87-112">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="96f87-112">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="96f87-113">ASP.NET MVC 4 的 WPI 安裝程式</span><span class="sxs-lookup"><span data-stu-id="96f87-113">WPI installer for ASP.NET MVC 4</span></span>](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [<span data-ttu-id="96f87-114">LocalDB</span><span class="sxs-lookup"><span data-stu-id="96f87-114">LocalDB</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [<span data-ttu-id="96f87-115">SSDT</span><span class="sxs-lookup"><span data-stu-id="96f87-115">SSDT</span></span>](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> <span data-ttu-id="96f87-116">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請安裝[WPI ASP.NET MVC 4 的安裝程式](https://go.microsoft.com/fwlink/?LinkId=243392)而： [Visual Studio 2010 的必要元件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)</span><span class="sxs-lookup"><span data-stu-id="96f87-116">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the [WPI installer for ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) and the: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)</span></span>
>
> <span data-ttu-id="96f87-117">使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="96f87-117">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="96f87-118">[下載 C# 版本](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)。</span><span class="sxs-lookup"><span data-stu-id="96f87-118">[Download the C# version](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).</span></span>
>
> <span data-ttu-id="96f87-119">在本教學課程中，您可以執行應用程式在 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="96f87-119">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="96f87-120">您也可以提供應用程式透過網際網路將它部署至主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="96f87-120">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="96f87-121">Microsoft 提供免費的 web 裝載中的最多 10 個 web sites[免費的 Windows Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="96f87-121">Microsoft offers free web hosting for up to 10 web sites in a [free Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="96f87-122">如需有關如何將 Visual Studio web 專案部署至 Windows Azure 網站的資訊，請參閱 <<c0> [ 建立及部署 ASP.NET 網站和 SQL Database，使用 Visual Studio](https://docs.microsoft.com/dotnet/azure/)。</span><span class="sxs-lookup"><span data-stu-id="96f87-122">For information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create and deploy an ASP.NET web site and SQL Database with Visual Studio](https://docs.microsoft.com/dotnet/azure/).</span></span> <span data-ttu-id="96f87-123">該教學課程也會示範如何使用 Entity Framework Code First Migrations，將 SQL Server 資料庫部署到 Windows Azure SQL Database (先前稱為 SQL Azure)。</span><span class="sxs-lookup"><span data-stu-id="96f87-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Windows Azure SQL Database (formerly SQL Azure).</span></span>
>
> <span data-ttu-id="96f87-124">本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="96f87-124">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="96f87-125">您將建置</span><span class="sxs-lookup"><span data-stu-id="96f87-125">What You'll Build</span></span>

> [!NOTE]
> <span data-ttu-id="96f87-126">如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)。</span><span class="sxs-lookup"><span data-stu-id="96f87-126">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="96f87-127">新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。</span><span class="sxs-lookup"><span data-stu-id="96f87-127">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>


<span data-ttu-id="96f87-128">您將實作簡單的電影清單應用程式可支援建立、 編輯、 搜尋和列出資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="96f87-128">You'll implement a simple movie-listing application that supports creating, editing, searching and listing movies from a database.</span></span> <span data-ttu-id="96f87-129">以下是兩個螢幕擷取畫面，您將建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="96f87-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="96f87-130">它包含顯示資料庫中的電影清單的頁面：</span><span class="sxs-lookup"><span data-stu-id="96f87-130">It includes a page that displays a list of movies from a database:</span></span>

![](intro-to-aspnet-mvc-4/_static/image1.png)

<span data-ttu-id="96f87-131">應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的項目相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="96f87-131">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="96f87-132">所有的輸入資料的案例包括驗證，以確保儲存在資料庫中的資料正確無誤。</span><span class="sxs-lookup"><span data-stu-id="96f87-132">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a><span data-ttu-id="96f87-133">快速入門</span><span class="sxs-lookup"><span data-stu-id="96f87-133">Getting Started</span></span>

<span data-ttu-id="96f87-134">從執行 Visual Studio Express 2012 或 Visual Web Developer 2010 Express 開始。</span><span class="sxs-lookup"><span data-stu-id="96f87-134">Start by running Visual Studio Express 2012 or Visual Web Developer 2010 Express.</span></span> <span data-ttu-id="96f87-135">大部分的螢幕擷取畫面，在此系列使用 Visual Studio Express 2012，但您可以完成本教學課程，Visual Studio 2010 SP1 中，Visual Studio 2012、 Visual Studio Express 2012 或 Visual Web Developer 2010 Express。</span><span class="sxs-lookup"><span data-stu-id="96f87-135">Most of the screen shots in this series use Visual Studio Express 2012, but you can complete this tutorial with Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 or Visual Web Developer 2010 Express.</span></span> <span data-ttu-id="96f87-136">選取 **新的專案**從**開始**頁面。</span><span class="sxs-lookup"><span data-stu-id="96f87-136">Select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="96f87-137">Visual Studio 是 IDE 或整合式的開發環境。</span><span class="sxs-lookup"><span data-stu-id="96f87-137">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="96f87-138">就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="96f87-138">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="96f87-139">在 Visual Studio 沒有工具列上方顯示各種可用的選項給您。</span><span class="sxs-lookup"><span data-stu-id="96f87-139">In Visual Studio there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="96f87-140">另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。</span><span class="sxs-lookup"><span data-stu-id="96f87-140">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="96f87-141">(例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)</span><span class="sxs-lookup"><span data-stu-id="96f87-141">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="96f87-142">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="96f87-142">Creating Your First Application</span></span>

<span data-ttu-id="96f87-143">您可以建立使用 Visual Basic 或 Visual C# 程式設計語言的應用程式。</span><span class="sxs-lookup"><span data-stu-id="96f87-143">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="96f87-144">選取 Visual C# 在左邊，然後選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="96f87-144">Select Visual C# on the left and then select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="96f87-145">命名您的專案&quot;MvcMovie&quot; ，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="96f87-145">Name your project &quot;MvcMovie&quot; and then click **OK**.</span></span>

![](intro-to-aspnet-mvc-4/_static/image4.png)

<span data-ttu-id="96f87-146">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="96f87-146">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="96f87-147">離開**Razor**做為預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="96f87-147">Leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-4/_static/image5.png)

<span data-ttu-id="96f87-148">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="96f87-148">Click **OK**.</span></span> <span data-ttu-id="96f87-149">Visual Studio 使用您剛才建立的 ASP.NET MVC 專案預設範本，因此您有運作中應用程式現在無須執行任何動作 ！</span><span class="sxs-lookup"><span data-stu-id="96f87-149">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="96f87-150">這是一項簡單&quot;Hello World ！&quot;專案和它的啟動您的應用程式的好地方。</span><span class="sxs-lookup"><span data-stu-id="96f87-150">This is a simple &quot;Hello World!&quot; project, and it's a good place to start your application.</span></span>

![](intro-to-aspnet-mvc-4/_static/image6.png)

<span data-ttu-id="96f87-151">從 [偵錯] 功能表中，選取 [開始偵錯]。</span><span class="sxs-lookup"><span data-stu-id="96f87-151">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-4/_static/image7.png)

<span data-ttu-id="96f87-152">請注意，若要開始偵錯的鍵盤快速鍵 F5。</span><span class="sxs-lookup"><span data-stu-id="96f87-152">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="96f87-153">F5 會導致 Visual Studio 啟動 IIS Express，並執行您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="96f87-153">F5 causes Visual Studio to start IIS Express and run your web application.</span></span> <span data-ttu-id="96f87-154">Visual Studio 接著會啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="96f87-154">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="96f87-155">通知，指出瀏覽器的網址列`localhost`而不是類似`example.com`。</span><span class="sxs-lookup"><span data-stu-id="96f87-155">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="96f87-156">這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="96f87-156">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="96f87-157">Visual Studio 執行時 web 專案，會將隨機的連接埠用於 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="96f87-157">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="96f87-158">下圖中的連接埠號碼會是 41788。</span><span class="sxs-lookup"><span data-stu-id="96f87-158">In the image below, the port number is 41788.</span></span> <span data-ttu-id="96f87-159">當您執行應用程式時，您可能會看到不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="96f87-159">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-4/_static/image8.png)

<span data-ttu-id="96f87-160">現成這個預設範本可讓您住家、 連絡人和相關頁面。</span><span class="sxs-lookup"><span data-stu-id="96f87-160">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="96f87-161">它也提供支援，以註冊和登入，並連結至 Facebook 和 Twitter。</span><span class="sxs-lookup"><span data-stu-id="96f87-161">It also provides support to register and log in, and links to Facebook and Twitter.</span></span> <span data-ttu-id="96f87-162">下一個步驟是要變更此應用程式的運作方式有點了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="96f87-162">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="96f87-163">關閉瀏覽器，並讓我們變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="96f87-163">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="96f87-164">下一步</span><span class="sxs-lookup"><span data-stu-id="96f87-164">Next</span></span>](adding-a-controller.md)
