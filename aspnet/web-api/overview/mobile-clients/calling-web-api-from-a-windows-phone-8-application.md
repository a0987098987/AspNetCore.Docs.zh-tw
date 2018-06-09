---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: 呼叫 Web API，從 Windows Phone 8 應用程式 (C#) |Microsoft 文件
author: rmcmurray
description: 建立完整的端對端案例，提供給 Windows Phone 8 應用程式的書籍目錄的 ASP.NET Web API 應用程式所組成。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874222"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="ac41d-103">從 Windows Phone 8 應用程式 (C#) 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="ac41d-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="ac41d-104">由[Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="ac41d-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="ac41d-105">在本教學課程中，您將學習如何建立完整的端對端案例，提供給 Windows Phone 8 應用程式的書籍目錄的 ASP.NET Web API 應用程式所組成。</span><span class="sxs-lookup"><span data-stu-id="ac41d-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="ac41d-106">總覽</span><span class="sxs-lookup"><span data-stu-id="ac41d-106">Overview</span></span>

<span data-ttu-id="ac41d-107">Rest 式服務，例如 ASP.NET Web API 伺服器端和用戶端應用程式的架構，藉以簡化建立以 HTTP 為基礎的應用程式開發人員。</span><span class="sxs-lookup"><span data-stu-id="ac41d-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="ac41d-108">而不是建立專屬通訊端通訊協定，通訊，Web API 的開發人員只需要發行他們的應用程式必要的 HTTP 方法 (例如： GET、 POST、 PUT、 DELETE)，而且用戶端應用程式開發人員只需要使用所需的應用程式的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="ac41d-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="ac41d-109">在此端對端教學課程中，您將學習如何使用 Web API 來建立下列專案：</span><span class="sxs-lookup"><span data-stu-id="ac41d-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="ac41d-110">在[本教學課程的第一個部分](#STEP1)，您將建立來管理書籍目錄的建立、 讀取、 更新和刪除 (CRUD) 作業的所有支援的 ASP.NET Web API 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ac41d-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="ac41d-111">此應用程式將會使用[範例 XML 檔案 (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx)從 MSDN。</span><span class="sxs-lookup"><span data-stu-id="ac41d-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="ac41d-112">在[本教學課程的第二個](#STEP2)，您將建立互動式的 Windows Phone 8 應用程式從您的 Web API 應用程式擷取資料。</span><span class="sxs-lookup"><span data-stu-id="ac41d-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="ac41d-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="ac41d-113">Prerequisites</span></span>

- <span data-ttu-id="ac41d-114">Visual Studio 2013 安裝的 Windows Phone 8 SDK</span><span class="sxs-lookup"><span data-stu-id="ac41d-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="ac41d-115">Windows 8 或更新版本上安裝 HYPER-V 的 64 位元系統</span><span class="sxs-lookup"><span data-stu-id="ac41d-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="ac41d-116">如需其他需求，請參閱*系統需求*區段[Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471)下載頁面。</span><span class="sxs-lookup"><span data-stu-id="ac41d-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="ac41d-117">如果您要測試 Web 應用程式開發介面和您的本機系統上的 Windows Phone 8 專案之間的連線，您必須依照*[連接到 Web API 的應用程式在本機上的 Windows Phone 8 模擬器電腦](https://go.microsoft.com/fwlink/?LinkId=324014)* 文件，以設定測試環境。</span><span class="sxs-lookup"><span data-stu-id="ac41d-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="ac41d-118">步驟 1： 建立 Web 應用程式開發介面 Bookstore 專案</span><span class="sxs-lookup"><span data-stu-id="ac41d-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="ac41d-119">本端對端教學課程的第一個步驟是建立支援的 CRUD 作業; 所有的 Web API 專案請注意，您會將 Windows Phone 應用程式專案加入這個方案中[步驟 2](#STEP2)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="ac41d-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="ac41d-120">開啟**Visual Studio 2013**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="ac41d-121">按一下**檔案**，然後**新**，然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="ac41d-122">當**新專案** 對話方塊隨即顯示，接著展開**已安裝**，然後**範本**，然後**Visual C#**，，然後**Web**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="ac41d-123">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="ac41d-124">反白顯示**ASP.NET Web 應用程式**，輸入**BookStore**為專案名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="ac41d-125">當**新增 ASP.NET 專案**對話方塊隨即出現，請選取**Web API**範本，然後再按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="ac41d-126">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="ac41d-127">當 Web API 專案開啟時，請從專案移除範例控制器：</span><span class="sxs-lookup"><span data-stu-id="ac41d-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="ac41d-128">展開**控制器**在 [方案總管] 中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ac41d-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="ac41d-129">以滑鼠右鍵按一下**ValuesController.cs**檔案，然後再按一下**刪除**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="ac41d-130">按一下**確定**當系統提示您確認刪除。</span><span class="sxs-lookup"><span data-stu-id="ac41d-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="ac41d-131">將 XML 資料檔加入至 Web API 專案。這個檔案包含 bookstore 目錄的內容：</span><span class="sxs-lookup"><span data-stu-id="ac41d-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="ac41d-132">以滑鼠右鍵按一下**應用程式\_資料**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **新項目**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="ac41d-133">當**加入新項目**對話方塊隨即出現，請反白**XML 檔案**範本。</span><span class="sxs-lookup"><span data-stu-id="ac41d-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="ac41d-134">將檔案命名**Books.xml**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="ac41d-135">當**Books.xml**開啟檔案、 程式碼檔案中的取代的範例 XML **books.xml** MSDN 上的檔案：</span><span class="sxs-lookup"><span data-stu-id="ac41d-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="ac41d-136">儲存並關閉 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="ac41d-137">Bookstore 模型加入至 Web API 專案;此模型包含 bookstore 應用程式的建立、 讀取、 更新和刪除 (CRUD) 邏輯：</span><span class="sxs-lookup"><span data-stu-id="ac41d-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="ac41d-138">以滑鼠右鍵按一下**模型**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **類別**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="ac41d-139">當**加入新項目**] 對話方塊隨即顯示，將類別檔案**BookDetails.cs**，然後按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="ac41d-140">當**BookDetails.cs**開啟檔案、 檔案中的程式碼替換成下列：</span><span class="sxs-lookup"><span data-stu-id="ac41d-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="ac41d-141">儲存並關閉**BookDetails.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="ac41d-142">將 bookstore 控制器加入至 Web API 專案：</span><span class="sxs-lookup"><span data-stu-id="ac41d-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="ac41d-143">以滑鼠右鍵按一下**控制器**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **控制器**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="ac41d-144">當**新增 Scaffold**對話方塊隨即出現，請反白**Web API 2 控制器空白**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="ac41d-145">當**加入控制器**] 對話方塊隨即顯示，接著控制器**BooksController**，然後按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="ac41d-146">當**BooksController.cs**開啟檔案、 檔案中的程式碼替換成下列：</span><span class="sxs-lookup"><span data-stu-id="ac41d-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="ac41d-147">儲存並關閉**BooksController.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="ac41d-148">建置的 Web API 應用程式檢查有錯誤。</span><span class="sxs-lookup"><span data-stu-id="ac41d-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="ac41d-149">步驟 2： 加入 Windows Phone 8 Bookstore 目錄專案</span><span class="sxs-lookup"><span data-stu-id="ac41d-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="ac41d-150">此端對端案例中的下一個步驟是建立 Windows Phone 8 目錄應用程式。</span><span class="sxs-lookup"><span data-stu-id="ac41d-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="ac41d-151">此應用程式將會使用*Windows Phone 的資料繫結應用程式*範本預設的使用者介面，而且會使用您在建立 Web API 應用程式[步驟 1](#STEP1)本教學課程做為資料來源。</span><span class="sxs-lookup"><span data-stu-id="ac41d-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="ac41d-152">以滑鼠右鍵按一下**BookStore**方案中的在 方案總管 中，然後按一下 **新增**，然後**新專案**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="ac41d-153">當**新專案** 對話方塊隨即顯示，接著展開**已安裝**，然後**Visual C#**，然後**Windows Phone**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="ac41d-154">反白顯示**Windows Phone 的資料繫結應用程式**，輸入**BookCatalog**為名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="ac41d-155">將此 Json.NET NuGet 封裝加入**BookCatalog**專案：</span><span class="sxs-lookup"><span data-stu-id="ac41d-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="ac41d-156">以滑鼠右鍵按一下**參考**如**BookCatalog**專案在方案總管 中，然後按一下**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="ac41d-157">當**管理 NuGet 封裝** 對話方塊隨即顯示，接著展開**線上**區段，並反白顯示**nuget.org**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="ac41d-158">輸入**Json.NET**在搜尋 欄位，然後按一下搜尋圖示。</span><span class="sxs-lookup"><span data-stu-id="ac41d-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="ac41d-159">反白顯示**Json.NET**中搜尋結果中，然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="ac41d-160">當安裝完成後時，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="ac41d-161">新增**BookDetails**模型到**BookCatalog**專案; 這個項目包含 bookstore 類別的一般模型：</span><span class="sxs-lookup"><span data-stu-id="ac41d-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="ac41d-162">以滑鼠右鍵按一下**BookCatalog**方案總管 中的專案，然後按一下 **新增**，然後按一下 **新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="ac41d-163">將新的資料夾命名**模型**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="ac41d-164">以滑鼠右鍵按一下**模型**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **類別**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="ac41d-165">當**加入新項目**] 對話方塊隨即顯示，將類別檔案**BookDetails.cs**，然後按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="ac41d-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="ac41d-166">當**BookDetails.cs**開啟檔案、 檔案中的程式碼替換成下列：</span><span class="sxs-lookup"><span data-stu-id="ac41d-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="ac41d-167">儲存並關閉**BookDetails.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="ac41d-168">更新**MainViewModel.cs**類別，以包含 BookStore Web API 應用程式進行通訊的功能：</span><span class="sxs-lookup"><span data-stu-id="ac41d-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="ac41d-169">展開**ViewModels**方案總管 中，然後按兩下資料夾**MainViewModel.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="ac41d-170">當**MainViewModel.cs**開啟檔案時，取代為下列程式碼檔案中的; 請注意，您將需要更新的值`apiUrl`常數，其 Web API 的實際 URL:</span><span class="sxs-lookup"><span data-stu-id="ac41d-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="ac41d-171">儲存並關閉**MainViewModel.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="ac41d-172">更新**MainPage.xaml**檔案以自訂應用程式名稱：</span><span class="sxs-lookup"><span data-stu-id="ac41d-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="ac41d-173">按兩下**MainPage.xaml**在 [方案總管] 中的檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="ac41d-174">當**MainPage.xaml**開啟檔案時，找出下列幾行程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac41d-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="ac41d-175">以下列內容取代這些行：</span><span class="sxs-lookup"><span data-stu-id="ac41d-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="ac41d-176">儲存並關閉**MainPage.xaml**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="ac41d-177">更新**DetailsPage.xaml**檔案來自訂顯示的項目：</span><span class="sxs-lookup"><span data-stu-id="ac41d-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="ac41d-178">按兩下**DetailsPage.xaml**在 [方案總管] 中的檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="ac41d-179">當**DetailsPage.xaml**開啟檔案時，找出下列幾行程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac41d-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="ac41d-180">以下列內容取代這些行：</span><span class="sxs-lookup"><span data-stu-id="ac41d-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="ac41d-181">儲存並關閉**DetailsPage.xaml**檔案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="ac41d-182">建置 Windows Phone 應用程式檢查有錯誤。</span><span class="sxs-lookup"><span data-stu-id="ac41d-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="ac41d-183">步驟 3： 測試端對端解決方案</span><span class="sxs-lookup"><span data-stu-id="ac41d-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="ac41d-184">中所述*必要條件*> 一節，本教學課程，您會測試 Web 應用程式開發介面和 Windows Phone 8 之間的連線時的專案在本機系統上，您就必須依照中的指示 *[連接到本機電腦上的 Web API 應用程式的 Windows Phone 8 模擬器](https://go.microsoft.com/fwlink/?LinkId=324014)* 文件，以設定測試環境。</span><span class="sxs-lookup"><span data-stu-id="ac41d-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="ac41d-185">設定測試環境之後，您必須將 Windows Phone 應用程式設定為啟始專案。</span><span class="sxs-lookup"><span data-stu-id="ac41d-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="ac41d-186">若要這樣做，請反白顯示**BookCatalog**應用程式在方案總管 中，然後按一下**設定為啟始專案**:</span><span class="sxs-lookup"><span data-stu-id="ac41d-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="ac41d-187">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-187">Click image to expand</span></span> |

<span data-ttu-id="ac41d-188">當您按下 F5 時，Visual Studio 會啟動兩個 Windows Phone 模擬器中，將會顯示&quot;稍候&quot;訊息時的應用程式資料會從您的 Web API:</span><span class="sxs-lookup"><span data-stu-id="ac41d-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="ac41d-189">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-189">Click image to expand</span></span> |

<span data-ttu-id="ac41d-190">如果所有項目成功，您應該會看到顯示類別目錄：</span><span class="sxs-lookup"><span data-stu-id="ac41d-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="ac41d-191">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-191">Click image to expand</span></span> |

<span data-ttu-id="ac41d-192">如果您點選任何書名，應用程式會顯示活頁簿描述：</span><span class="sxs-lookup"><span data-stu-id="ac41d-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="ac41d-193">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-193">Click image to expand</span></span> |

<span data-ttu-id="ac41d-194">如果應用程式無法與您的 Web API 通訊，就會顯示錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="ac41d-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="ac41d-195">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-195">Click image to expand</span></span> |

<span data-ttu-id="ac41d-196">如果您點選 錯誤訊息上，將會顯示任何錯誤的其他詳細資料：</span><span class="sxs-lookup"><span data-stu-id="ac41d-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="ac41d-197">按一下以展開的影像</span><span class="sxs-lookup"><span data-stu-id="ac41d-197">Click image to expand</span></span>                                                                 |

