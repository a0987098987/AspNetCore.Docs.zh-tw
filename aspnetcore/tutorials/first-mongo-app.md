---
title: 使用 ASP.NET Core 與 MongoDB 建立 Web API
author: prkhandelwal
description: 此教學課程示範如何使用 MongoDB NoSQL 資料庫建立 ASP.NET Core Web API。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-mongo-app
ms.openlocfilehash: 831479f04551441b079d3f34d043c7486bad7ac0
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409016"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="e4bec-103">使用 ASP.NET Core 與 MongoDB 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="e4bec-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="e4bec-104">作者：[Pratik Khandelwal](https://twitter.com/K2Prk) 與 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e4bec-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e4bec-105">此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="e4bec-106">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="e4bec-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4bec-107">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-107">Configure MongoDB</span></span>
> * <span data-ttu-id="e4bec-108">建立 MongoDB 資料庫</span><span class="sxs-lookup"><span data-stu-id="e4bec-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="e4bec-109">定義 MongoDB 集合與結構描述</span><span class="sxs-lookup"><span data-stu-id="e4bec-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="e4bec-110">從 Web API 執行 MongoDB CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="e4bec-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="e4bec-111">自訂 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="e4bec-111">Customize JSON serialization</span></span>

<span data-ttu-id="e4bec-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="e4bec-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4bec-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="e4bec-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4bec-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4bec-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="e4bec-115">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e4bec-115">.NET Core SDK 3.0 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* <span data-ttu-id="e4bec-116">**ASP.NET 和 網頁程式開發**工作負載的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="e4bec-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e4bec-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4bec-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="e4bec-119">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e4bec-119">.NET Core SDK 3.0 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* [<span data-ttu-id="e4bec-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e4bec-121">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [<span data-ttu-id="e4bec-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4bec-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4bec-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="e4bec-124">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e4bec-124">.NET Core SDK 3.0 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* [<span data-ttu-id="e4bec-125">Visual Studio for Mac 7.7 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="e4bec-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="e4bec-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="e4bec-127">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-127">Configure MongoDB</span></span>

<span data-ttu-id="e4bec-128">若使用 Windows，MongoDB 預設會安裝在 *C:\\Program Files\\MongoDB*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="e4bec-129">將*C： \\ Program Files \\ MongoDB \\ 伺服器 \\ \<version_number> \\ bin*新增至 `Path` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="e4bec-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="e4bec-130">此變更會啟用從您開發機器上的任意位置存取 MongoDB 的功能。</span><span class="sxs-lookup"><span data-stu-id="e4bec-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="e4bec-131">在下列步驟中使用 mongo 殼層來建立資料庫、建立集合及存放文件。</span><span class="sxs-lookup"><span data-stu-id="e4bec-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="e4bec-132">如需有關 mongo 殼層命令的詳細資訊，請參閱[使用 mongo 殼層](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)。</span><span class="sxs-lookup"><span data-stu-id="e4bec-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="e4bec-133">選擇您開發機器上的目錄來存放資料。</span><span class="sxs-lookup"><span data-stu-id="e4bec-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="e4bec-134">例如， Windows 上的 *C:\\BooksData*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="e4bec-135">若該目錄不存在，請建立它。</span><span class="sxs-lookup"><span data-stu-id="e4bec-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="e4bec-136">mongo 殼層不會建立新目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="e4bec-137">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="e4bec-137">Open a command shell.</span></span> <span data-ttu-id="e4bec-138">執行下列命令以連線到預設連接埠 27017 上的 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e4bec-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="e4bec-139">請記得將 `<data_directory_path>` 取代為您在上一個步驟中選擇的目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="e4bec-140">開啟另一個命令殼層執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4bec-140">Open another command shell instance.</span></span> <span data-ttu-id="e4bec-141">執行下列命令以連線到預設測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="e4bec-141">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="e4bec-142">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4bec-142">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="e4bec-143">若它不存在，會建立名為 *BookstoreDb* 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4bec-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="e4bec-144">若該資料庫存在，會開啟其連線進行交易。</span><span class="sxs-lookup"><span data-stu-id="e4bec-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="e4bec-145">使用下列命令建立 `Books` 集合：</span><span class="sxs-lookup"><span data-stu-id="e4bec-145">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="e4bec-146">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="e4bec-146">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="e4bec-147">使用下列命令為 `Books` 集合定義結構描述並插入兩份文件：</span><span class="sxs-lookup"><span data-stu-id="e4bec-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="e4bec-148">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="e4bec-148">The following result is displayed:</span></span>

   ```console
   {
     "acknowledged" : true,
     "insertedIds" : [
       ObjectId("5bfd996f7b8e48dc15ff215d"),
       ObjectId("5bfd996f7b8e48dc15ff215e")
     ]
   }
   ```
  
   > [!NOTE]
   > <span data-ttu-id="e4bec-149">您執行此範例時的識別碼，將不同於本文中顯示的識別碼。</span><span class="sxs-lookup"><span data-stu-id="e4bec-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="e4bec-150">使用下列命令檢視資料庫中的文件：</span><span class="sxs-lookup"><span data-stu-id="e4bec-150">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="e4bec-151">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="e4bec-151">The following result is displayed:</span></span>

   ```console
   {
     "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
     "Name" : "Design Patterns",
     "Price" : 54.93,
     "Category" : "Computers",
     "Author" : "Ralph Johnson"
   }
   {
     "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
     "Name" : "Clean Code",
     "Price" : 43.15,
     "Category" : "Computers",
     "Author" : "Robert C. Martin"
   }
   ```

   <span data-ttu-id="e4bec-152">結構描述會為每份文件新增自動彙總的 `_id` 屬性 (類型 `ObjectId`)。</span><span class="sxs-lookup"><span data-stu-id="e4bec-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="e4bec-153">資料庫已就緒。</span><span class="sxs-lookup"><span data-stu-id="e4bec-153">The database is ready.</span></span> <span data-ttu-id="e4bec-154">您可以開始建立 ASP.NET Core Web API。</span><span class="sxs-lookup"><span data-stu-id="e4bec-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="e4bec-155">建立 ASP.NET Core Web API 專案</span><span class="sxs-lookup"><span data-stu-id="e4bec-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4bec-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4bec-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e4bec-157">移至 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="e4bec-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="e4bec-158">選取 [ASP.NET Core Web 應用程式]\*\*\*\* 專案類型，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="e4bec-159">將專案命名為 *BooksApi*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="e4bec-160">選取 [.NET Core]\*\*\*\* 目標架構與 [ASP.NET Core 3.0]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-160">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="e4bec-161">選取 [API]\*\*\*\* 專案範本，然後選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="e4bec-162">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e4bec-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="e4bec-163">在 [套件管理員主控台]\*\*\*\* 視窗中，瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="e4bec-164">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="e4bec-164">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4bec-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e4bec-166">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4bec-166">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="e4bec-167">會產生以 .NET Core 為目標的新 ASP.NET Core Web API 專案，並在 Visual Studio Code 中開啟。</span><span class="sxs-lookup"><span data-stu-id="e4bec-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="e4bec-168">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' BooksApi ' 中遺漏了 debug。加入它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="e4bec-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="e4bec-169">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-169">Select **Yes**.</span></span>
1. <span data-ttu-id="e4bec-170">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e4bec-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="e4bec-171">開啟 [整合式終端機]\*\*\*\* 並瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="e4bec-172">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="e4bec-172">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4bec-173">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4bec-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="e4bec-174">在8.6 版之前的 Visual Studio for Mac 中， **File**  >  從邊欄選取 [檔案] [**新增方案**]  >  [**.net Core**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="e4bec-174">In Visual Studio for Mac earlier than version 8.6, select **File** > **New Solution** > **.NET Core** > **App** from the sidebar.</span></span> <span data-ttu-id="e4bec-175">在8.6 版或更新版本中**File**，  >  從邊欄選取 [檔案] [**新增方案**]  >  [**Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="e4bec-175">In version 8.6 or later, select **File** > **New Solution** > **Web and Console** > **App** from the sidebar.</span></span>
1. <span data-ttu-id="e4bec-176">選取 [ **ASP.NET Core** > **API** c #] 專案範本，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="e4bec-176">Select the **ASP.NET Core** > **API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="e4bec-177">從 [**目標 Framework** ] 下拉式清單中選取 [ **.Net Core 3.1** ]，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="e4bec-177">Select **.NET Core 3.1** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="e4bec-178">在 [專案名稱]\*\*\*\* 中輸入 *BooksApi*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-178">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="e4bec-179">在 [方案]\*\*\*\* 台中，以滑鼠右鍵按一下專案的 [相依性]\*\*\*\* 節點並選取 [新增封裝]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-179">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="e4bec-180">在搜尋方塊中輸入 *MongoDB.Driver*，然後依序選取 *MongoDB.Driver* 套件和 [新增套件]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-180">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="e4bec-181">選取 [授權接受]\*\*\*\* 對話方塊中的 [接受]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e4bec-181">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="e4bec-182">新增實體模型</span><span class="sxs-lookup"><span data-stu-id="e4bec-182">Add an entity model</span></span>

1. <span data-ttu-id="e4bec-183">新增 *Models* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-183">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="e4bec-184">新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-184">Add a `Book` class to the *Models* directory with the following code:</span></span>

   ```csharp
   using MongoDB.Bson;
   using MongoDB.Bson.Serialization.Attributes;

   namespace BooksApi.Models
   {
       public class Book
       {
           [BsonId]
           [BsonRepresentation(BsonType.ObjectId)]
           public string Id { get; set; }

           [BsonElement("Name")]
           public string BookName { get; set; }

           public decimal Price { get; set; }

           public string Category { get; set; }

           public string Author { get; set; }
       }
   }
   ```

   <span data-ttu-id="e4bec-185">在上面的類別中，需要 `Id` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e4bec-185">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="e4bec-186">才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。</span><span class="sxs-lookup"><span data-stu-id="e4bec-186">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="e4bec-187">已標注， [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) 可將此屬性指定為檔的主要金鑰。</span><span class="sxs-lookup"><span data-stu-id="e4bec-187">Is annotated with [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="e4bec-188">會以標注， [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) 允許以類型 `string` （而非[ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm)結構）傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="e4bec-188">Is annotated with [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="e4bec-189">Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-189">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="e4bec-190">`BookName`屬性會以 [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) 屬性標注。</span><span class="sxs-lookup"><span data-stu-id="e4bec-190">The `BookName` property is annotated with the [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="e4bec-191">`Name` 的屬性值代表 MongoDB 集合中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-191">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="e4bec-192">新增組態模型</span><span class="sxs-lookup"><span data-stu-id="e4bec-192">Add a configuration model</span></span>

1. <span data-ttu-id="e4bec-193">將下列資料庫組態值新增至 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="e4bec-193">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="e4bec-194">使用下列程式碼將 *BookstoreDatabaseSettings.cs* 檔案新增至 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-194">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="e4bec-195">上述 `BookstoreDatabaseSettings` 類別用來儲存 *appsettings.json* 檔案的 `BookstoreDatabaseSettings` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="e4bec-195">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="e4bec-196">JSON 和 C# 屬性名稱以相同方式命名，以簡化對應程序。</span><span class="sxs-lookup"><span data-stu-id="e4bec-196">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="e4bec-197">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="e4bec-197">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-8)]

   <span data-ttu-id="e4bec-198">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="e4bec-198">In the preceding code:</span></span>

   * <span data-ttu-id="e4bec-199">*appsettings.json* 檔案之 `BookstoreDatabaseSettings` 區段所繫結的組態執行個體，是在相依性插入 (DI) 容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="e4bec-199">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="e4bec-200">例如，`BookstoreDatabaseSettings` 物件的 `ConnectionString` 屬性會填入 *appsettings.json* 中的 `BookstoreDatabaseSettings:ConnectionString` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e4bec-200">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="e4bec-201">`IBookstoreDatabaseSettings` 介面使用 singleton [服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)在 DI 中註冊。</span><span class="sxs-lookup"><span data-stu-id="e4bec-201">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="e4bec-202">插入時，介面執行個體會解析成 `BookstoreDatabaseSettings` 物件。</span><span class="sxs-lookup"><span data-stu-id="e4bec-202">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="e4bec-203">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookstoreDatabaseSettings` 和 `IBookstoreDatabaseSettings` 參考：</span><span class="sxs-lookup"><span data-stu-id="e4bec-203">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="e4bec-204">新增 CRUD 作業服務</span><span class="sxs-lookup"><span data-stu-id="e4bec-204">Add a CRUD operations service</span></span>

1. <span data-ttu-id="e4bec-205">新增 *Services* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-205">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="e4bec-206">新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-206">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="e4bec-207">在上述程式碼中，透過建構函式插入從 DI 擷取 `IBookstoreDatabaseSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4bec-207">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="e4bec-208">這項技術可讓您存取[新增組態模型](#add-a-configuration-model)一節中已新增的 *appsettings.json* 組態值。</span><span class="sxs-lookup"><span data-stu-id="e4bec-208">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="e4bec-209">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="e4bec-209">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="e4bec-210">在上述程式碼中，`BookService` 類別要向 DI 註冊才能在取用類別中支援建構函式插入。</span><span class="sxs-lookup"><span data-stu-id="e4bec-210">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="e4bec-211">因為 `BookService` 直接依存於 `MongoClient`，所以 Singleton 服務存留期最適合。</span><span class="sxs-lookup"><span data-stu-id="e4bec-211">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="e4bec-212">根據官方的[Mongo 用戶端重複使用方針](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)， `MongoClient` 應該在 DI 中註冊單一服務存留期。</span><span class="sxs-lookup"><span data-stu-id="e4bec-212">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="e4bec-213">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookService` 參考：</span><span class="sxs-lookup"><span data-stu-id="e4bec-213">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="e4bec-214">`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：</span><span class="sxs-lookup"><span data-stu-id="e4bec-214">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="e4bec-215">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm)：讀取用來執行資料庫作業的伺服器實例。</span><span class="sxs-lookup"><span data-stu-id="e4bec-215">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm): Reads the server instance for performing database operations.</span></span> <span data-ttu-id="e4bec-216">此類別的建構函式是使用 MongoDB 連接字串提供：</span><span class="sxs-lookup"><span data-stu-id="e4bec-216">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="e4bec-217">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm)：代表用於執行作業的 Mongo 資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4bec-217">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm): Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="e4bec-218">本教學課程在介面中使用一般的 [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) 方法來存取特定集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="e4bec-218">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="e4bec-219">呼叫此方法之後，針對集合執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-219">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="e4bec-220">在 `GetCollection<TDocument>(collection)` 方法呼叫中：</span><span class="sxs-lookup"><span data-stu-id="e4bec-220">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="e4bec-221">`collection` 代表集合名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-221">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="e4bec-222">`TDocument` 代表儲存在集合中的 CLR 物件類型。</span><span class="sxs-lookup"><span data-stu-id="e4bec-222">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="e4bec-223">`GetCollection<TDocument>(collection)` 傳回代表集合的 [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) 物件。</span><span class="sxs-lookup"><span data-stu-id="e4bec-223">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="e4bec-224">在此教學課程中，會在集合上叫用下列方法：</span><span class="sxs-lookup"><span data-stu-id="e4bec-224">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="e4bec-225">[Collection.deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm)：刪除符合所提供搜尋條件的單一檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-225">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm): Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="e4bec-226">[尋找 \<TDocument> ](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm)：傳回集合中符合所提供搜尋條件的所有檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-226">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm): Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="e4bec-227">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm)：將提供的物件插入為集合中的新檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-227">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm): Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="e4bec-228">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm)：使用提供的物件取代符合所提供搜尋條件的單一檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-228">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm): Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="e4bec-229">新增控制器</span><span class="sxs-lookup"><span data-stu-id="e4bec-229">Add a controller</span></span>

<span data-ttu-id="e4bec-230">新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-230">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="e4bec-231">上述 Web API 控制器會：</span><span class="sxs-lookup"><span data-stu-id="e4bec-231">The preceding web API controller:</span></span>

* <span data-ttu-id="e4bec-232">使用 `BookService` 類別來執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-232">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="e4bec-233">包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="e4bec-233">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="e4bec-234">在 `Create` 動作方法中呼叫 <xref:System.Web.Http.ApiController.CreatedAtRoute*>，以傳回 [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 回應。</span><span class="sxs-lookup"><span data-stu-id="e4bec-234">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="e4bec-235">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是狀態碼 201。</span><span class="sxs-lookup"><span data-stu-id="e4bec-235">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="e4bec-236">`CreatedAtRoute` 也會將 `Location` 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="e4bec-236">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="e4bec-237">`Location` 標頭指定新建活頁簿的 URI。</span><span class="sxs-lookup"><span data-stu-id="e4bec-237">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="e4bec-238">測試 Web API</span><span class="sxs-lookup"><span data-stu-id="e4bec-238">Test the web API</span></span>

1. <span data-ttu-id="e4bec-239">建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4bec-239">Build and run the app.</span></span>

1. <span data-ttu-id="e4bec-240">巡覽至 `http://localhost:<port>/api/books`，測試控制器的無參數 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="e4bec-240">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="e4bec-241">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="e4bec-241">The following JSON response is displayed:</span></span>

   ```json
   [
     {
       "id":"5bfd996f7b8e48dc15ff215d",
       "bookName":"Design Patterns",
       "price":54.93,
       "category":"Computers",
       "author":"Ralph Johnson"
     },
     {
       "id":"5bfd996f7b8e48dc15ff215e",
       "bookName":"Clean Code",
       "price":43.15,
       "category":"Computers",
       "author":"Robert C. Martin"
     }
   ]
   ```

1. <span data-ttu-id="e4bec-242">巡覽至 `http://localhost:<port>/api/books/{id here}`，測試控制器的多載 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="e4bec-242">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="e4bec-243">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="e4bec-243">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="e4bec-244">設定 JSON 序列化選項</span><span class="sxs-lookup"><span data-stu-id="e4bec-244">Configure JSON serialization options</span></span>

<span data-ttu-id="e4bec-245">[測試 Web API](#test-the-web-api) 區段中傳回的 JSON 回應，有兩個詳細資訊要變更：</span><span class="sxs-lookup"><span data-stu-id="e4bec-245">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="e4bec-246">屬性名稱的預設駝峰式命名法大小寫應予變更，使其符合CLR 物件屬性名稱的 Pascal 命名法大小寫。</span><span class="sxs-lookup"><span data-stu-id="e4bec-246">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="e4bec-247">`bookName` 屬性應傳回為 `Name`。</span><span class="sxs-lookup"><span data-stu-id="e4bec-247">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="e4bec-248">為滿足上述需求，請進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="e4bec-248">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="e4bec-249">已從 ASP.NET 共用架構移除 JSON.NET。</span><span class="sxs-lookup"><span data-stu-id="e4bec-249">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="e4bec-250">將套件參考新增至 [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) 。</span><span class="sxs-lookup"><span data-stu-id="e4bec-250">Add a package reference to [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="e4bec-251">在 `Startup.ConfigureServices` 中，將下列醒目提示的程式碼鏈結至 `AddControllers` 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="e4bec-251">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddControllers` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="e4bec-252">完成前述變更後，Web API 序列化 JSON 回應中屬性名稱即符合其對應的 CLR 物件類型屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-252">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="e4bec-253">例如，`Book` 類別的 `Author` 屬性會序列化為 `Author`。</span><span class="sxs-lookup"><span data-stu-id="e4bec-253">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="e4bec-254">在*模型/Book .cs*中， `BookName` 使用下列屬性標注屬性 [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) ：</span><span class="sxs-lookup"><span data-stu-id="e4bec-254">In *Models/Book.cs*, annotate the `BookName` property with the following [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="e4bec-255">`[JsonProperty]` 的屬性值 `Name` 代表 Web API 序列化 JSON 回應中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-255">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="e4bec-256">在 *Models/Book.cs* 的頂端新增下列程式碼，以解析 `[JsonProperty]` 屬性參考：</span><span class="sxs-lookup"><span data-stu-id="e4bec-256">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="e4bec-257">重複[測試 Web API](#test-the-web-api) 一節中定義的步驟。</span><span class="sxs-lookup"><span data-stu-id="e4bec-257">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="e4bec-258">請注意 JSON 屬性名稱中的差異。</span><span class="sxs-lookup"><span data-stu-id="e4bec-258">Notice the difference in JSON property names.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e4bec-259">此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-259">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="e4bec-260">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="e4bec-260">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4bec-261">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-261">Configure MongoDB</span></span>
> * <span data-ttu-id="e4bec-262">建立 MongoDB 資料庫</span><span class="sxs-lookup"><span data-stu-id="e4bec-262">Create a MongoDB database</span></span>
> * <span data-ttu-id="e4bec-263">定義 MongoDB 集合與結構描述</span><span class="sxs-lookup"><span data-stu-id="e4bec-263">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="e4bec-264">從 Web API 執行 MongoDB CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="e4bec-264">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="e4bec-265">自訂 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="e4bec-265">Customize JSON serialization</span></span>

<span data-ttu-id="e4bec-266">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="e4bec-266">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4bec-267">必要條件</span><span class="sxs-lookup"><span data-stu-id="e4bec-267">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4bec-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4bec-268">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="e4bec-269">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="e4bec-269">.NET Core SDK 2.2</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* <span data-ttu-id="e4bec-270">**ASP.NET 和 網頁程式開發**工作負載的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="e4bec-270">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e4bec-271">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-271">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4bec-272">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-272">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="e4bec-273">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="e4bec-273">.NET Core SDK 2.2</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* [<span data-ttu-id="e4bec-274">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-274">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e4bec-275">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-275">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [<span data-ttu-id="e4bec-276">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-276">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4bec-277">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4bec-277">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="e4bec-278">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="e4bec-278">.NET Core SDK 2.2</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* [<span data-ttu-id="e4bec-279">Visual Studio for Mac 7.7 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="e4bec-279">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="e4bec-280">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-280">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="e4bec-281">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="e4bec-281">Configure MongoDB</span></span>

<span data-ttu-id="e4bec-282">若使用 Windows，MongoDB 預設會安裝在 *C:\\Program Files\\MongoDB*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-282">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="e4bec-283">將*C： \\ Program Files \\ MongoDB \\ 伺服器 \\ \<version_number> \\ bin*新增至 `Path` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="e4bec-283">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="e4bec-284">此變更會啟用從您開發機器上的任意位置存取 MongoDB 的功能。</span><span class="sxs-lookup"><span data-stu-id="e4bec-284">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="e4bec-285">在下列步驟中使用 mongo 殼層來建立資料庫、建立集合及存放文件。</span><span class="sxs-lookup"><span data-stu-id="e4bec-285">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="e4bec-286">如需有關 mongo 殼層命令的詳細資訊，請參閱[使用 mongo 殼層](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)。</span><span class="sxs-lookup"><span data-stu-id="e4bec-286">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="e4bec-287">選擇您開發機器上的目錄來存放資料。</span><span class="sxs-lookup"><span data-stu-id="e4bec-287">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="e4bec-288">例如， Windows 上的 *C:\\BooksData*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-288">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="e4bec-289">若該目錄不存在，請建立它。</span><span class="sxs-lookup"><span data-stu-id="e4bec-289">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="e4bec-290">mongo 殼層不會建立新目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-290">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="e4bec-291">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="e4bec-291">Open a command shell.</span></span> <span data-ttu-id="e4bec-292">執行下列命令以連線到預設連接埠 27017 上的 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e4bec-292">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="e4bec-293">請記得將 `<data_directory_path>` 取代為您在上一個步驟中選擇的目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-293">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="e4bec-294">開啟另一個命令殼層執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4bec-294">Open another command shell instance.</span></span> <span data-ttu-id="e4bec-295">執行下列命令以連線到預設測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="e4bec-295">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="e4bec-296">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4bec-296">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="e4bec-297">若它不存在，會建立名為 *BookstoreDb* 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4bec-297">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="e4bec-298">若該資料庫存在，會開啟其連線進行交易。</span><span class="sxs-lookup"><span data-stu-id="e4bec-298">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="e4bec-299">使用下列命令建立 `Books` 集合：</span><span class="sxs-lookup"><span data-stu-id="e4bec-299">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="e4bec-300">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="e4bec-300">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="e4bec-301">使用下列命令為 `Books` 集合定義結構描述並插入兩份文件：</span><span class="sxs-lookup"><span data-stu-id="e4bec-301">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="e4bec-302">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="e4bec-302">The following result is displayed:</span></span>

   ```console
   {
     "acknowledged" : true,
     "insertedIds" : [
       ObjectId("5bfd996f7b8e48dc15ff215d"),
       ObjectId("5bfd996f7b8e48dc15ff215e")
     ]
   }
   ```
  
   > [!NOTE]
   > <span data-ttu-id="e4bec-303">您執行此範例時的識別碼，將不同於本文中顯示的識別碼。</span><span class="sxs-lookup"><span data-stu-id="e4bec-303">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="e4bec-304">使用下列命令檢視資料庫中的文件：</span><span class="sxs-lookup"><span data-stu-id="e4bec-304">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="e4bec-305">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="e4bec-305">The following result is displayed:</span></span>

   ```console
   {
     "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
     "Name" : "Design Patterns",
     "Price" : 54.93,
     "Category" : "Computers",
     "Author" : "Ralph Johnson"
   }
   {
     "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
     "Name" : "Clean Code",
     "Price" : 43.15,
     "Category" : "Computers",
     "Author" : "Robert C. Martin"
   }
   ```

   <span data-ttu-id="e4bec-306">結構描述會為每份文件新增自動彙總的 `_id` 屬性 (類型 `ObjectId`)。</span><span class="sxs-lookup"><span data-stu-id="e4bec-306">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="e4bec-307">資料庫已就緒。</span><span class="sxs-lookup"><span data-stu-id="e4bec-307">The database is ready.</span></span> <span data-ttu-id="e4bec-308">您可以開始建立 ASP.NET Core Web API。</span><span class="sxs-lookup"><span data-stu-id="e4bec-308">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="e4bec-309">建立 ASP.NET Core Web API 專案</span><span class="sxs-lookup"><span data-stu-id="e4bec-309">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4bec-310">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4bec-310">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e4bec-311">移至 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="e4bec-311">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="e4bec-312">選取 [ASP.NET Core Web 應用程式]\*\*\*\* 專案類型，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-312">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="e4bec-313">將專案命名為 *BooksApi*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-313">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="e4bec-314">選取 [.NET Core]\*\*\*\* 目標架構與 [ASP.NET Core 2.2]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-314">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="e4bec-315">選取 [API]\*\*\*\* 專案範本，然後選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-315">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="e4bec-316">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e4bec-316">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="e4bec-317">在 [套件管理員主控台]\*\*\*\* 視窗中，瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-317">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="e4bec-318">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="e4bec-318">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4bec-319">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4bec-319">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e4bec-320">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4bec-320">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="e4bec-321">會產生以 .NET Core 為目標的新 ASP.NET Core Web API 專案，並在 Visual Studio Code 中開啟。</span><span class="sxs-lookup"><span data-stu-id="e4bec-321">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="e4bec-322">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' BooksApi ' 中遺漏了 debug。加入它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="e4bec-322">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="e4bec-323">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-323">Select **Yes**.</span></span>
1. <span data-ttu-id="e4bec-324">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e4bec-324">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="e4bec-325">開啟 [整合式終端機]\*\*\*\* 並瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-325">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="e4bec-326">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="e4bec-326">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4bec-327">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4bec-327">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="e4bec-328">在8.6 版之前的 Visual Studio for Mac 中， **File**  >  從邊欄選取 [檔案] [**新增方案**]  >  [**.net Core**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="e4bec-328">In Visual Studio for Mac earlier than version 8.6, select **File** > **New Solution** > **.NET Core** > **App** from the sidebar.</span></span> <span data-ttu-id="e4bec-329">在8.6 版或更新版本中**File**，  >  從邊欄選取 [檔案] [**新增方案**]  >  [**Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="e4bec-329">In version 8.6 or later, select **File** > **New Solution** > **Web and Console** > **App** from the sidebar.</span></span>
1. <span data-ttu-id="e4bec-330">選取 [ASP.NET Core Web API]\*\*\*\* C# 專案範本，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-330">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="e4bec-331">從 [目標 Framework]\*\*\*\* 下拉式清單選取 [.NET Core 2.2]\*\*\*\*，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-331">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="e4bec-332">在 [專案名稱]\*\*\*\* 中輸入 *BooksApi*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-332">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="e4bec-333">在 [方案]\*\*\*\* 台中，以滑鼠右鍵按一下專案的 [相依性]\*\*\*\* 節點並選取 [新增封裝]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-333">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="e4bec-334">在搜尋方塊中輸入 *MongoDB.Driver*，然後依序選取 *MongoDB.Driver* 套件和 [新增套件]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e4bec-334">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="e4bec-335">選取 [授權接受]\*\*\*\* 對話方塊中的 [接受]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e4bec-335">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="e4bec-336">新增實體模型</span><span class="sxs-lookup"><span data-stu-id="e4bec-336">Add an entity model</span></span>

1. <span data-ttu-id="e4bec-337">新增 *Models* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-337">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="e4bec-338">新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-338">Add a `Book` class to the *Models* directory with the following code:</span></span>

   ```csharp
   using MongoDB.Bson;
   using MongoDB.Bson.Serialization.Attributes;

   namespace BooksApi.Models
   {
       public class Book
       {
           [BsonId]
           [BsonRepresentation(BsonType.ObjectId)]
           public string Id { get; set; }

           [BsonElement("Name")]
           public string BookName { get; set; }

           public decimal Price { get; set; }

           public string Category { get; set; }

           public string Author { get; set; }
       }
   }
   ```

   <span data-ttu-id="e4bec-339">在上面的類別中，需要 `Id` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e4bec-339">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="e4bec-340">才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。</span><span class="sxs-lookup"><span data-stu-id="e4bec-340">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="e4bec-341">已標注， [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) 可將此屬性指定為檔的主要金鑰。</span><span class="sxs-lookup"><span data-stu-id="e4bec-341">Is annotated with [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="e4bec-342">會以標注， [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) 允許以類型 `string` （而非[ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm)結構）傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="e4bec-342">Is annotated with [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="e4bec-343">Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-343">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="e4bec-344">`BookName`屬性會以 [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) 屬性標注。</span><span class="sxs-lookup"><span data-stu-id="e4bec-344">The `BookName` property is annotated with the [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="e4bec-345">`Name` 的屬性值代表 MongoDB 集合中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-345">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="e4bec-346">新增組態模型</span><span class="sxs-lookup"><span data-stu-id="e4bec-346">Add a configuration model</span></span>

1. <span data-ttu-id="e4bec-347">將下列資料庫組態值新增至 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="e4bec-347">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="e4bec-348">使用下列程式碼將 *BookstoreDatabaseSettings.cs* 檔案新增至 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-348">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="e4bec-349">上述 `BookstoreDatabaseSettings` 類別用來儲存 *appsettings.json* 檔案的 `BookstoreDatabaseSettings` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="e4bec-349">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="e4bec-350">JSON 和 C# 屬性名稱以相同方式命名，以簡化對應程序。</span><span class="sxs-lookup"><span data-stu-id="e4bec-350">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="e4bec-351">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="e4bec-351">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="e4bec-352">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="e4bec-352">In the preceding code:</span></span>

   * <span data-ttu-id="e4bec-353">*appsettings.json* 檔案之 `BookstoreDatabaseSettings` 區段所繫結的組態執行個體，是在相依性插入 (DI) 容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="e4bec-353">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="e4bec-354">例如，`BookstoreDatabaseSettings` 物件的 `ConnectionString` 屬性會填入 *appsettings.json* 中的 `BookstoreDatabaseSettings:ConnectionString` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e4bec-354">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="e4bec-355">`IBookstoreDatabaseSettings` 介面使用 singleton [服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)在 DI 中註冊。</span><span class="sxs-lookup"><span data-stu-id="e4bec-355">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="e4bec-356">插入時，介面執行個體會解析成 `BookstoreDatabaseSettings` 物件。</span><span class="sxs-lookup"><span data-stu-id="e4bec-356">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="e4bec-357">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookstoreDatabaseSettings` 和 `IBookstoreDatabaseSettings` 參考：</span><span class="sxs-lookup"><span data-stu-id="e4bec-357">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="e4bec-358">新增 CRUD 作業服務</span><span class="sxs-lookup"><span data-stu-id="e4bec-358">Add a CRUD operations service</span></span>

1. <span data-ttu-id="e4bec-359">新增 *Services* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="e4bec-359">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="e4bec-360">新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-360">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="e4bec-361">在上述程式碼中，透過建構函式插入從 DI 擷取 `IBookstoreDatabaseSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4bec-361">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="e4bec-362">這項技術可讓您存取[新增組態模型](#add-a-configuration-model)一節中已新增的 *appsettings.json* 組態值。</span><span class="sxs-lookup"><span data-stu-id="e4bec-362">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="e4bec-363">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="e4bec-363">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="e4bec-364">在上述程式碼中，`BookService` 類別要向 DI 註冊才能在取用類別中支援建構函式插入。</span><span class="sxs-lookup"><span data-stu-id="e4bec-364">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="e4bec-365">因為 `BookService` 直接依存於 `MongoClient`，所以 Singleton 服務存留期最適合。</span><span class="sxs-lookup"><span data-stu-id="e4bec-365">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="e4bec-366">根據官方的[Mongo 用戶端重複使用方針](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)， `MongoClient` 應該在 DI 中註冊單一服務存留期。</span><span class="sxs-lookup"><span data-stu-id="e4bec-366">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="e4bec-367">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookService` 參考：</span><span class="sxs-lookup"><span data-stu-id="e4bec-367">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="e4bec-368">`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：</span><span class="sxs-lookup"><span data-stu-id="e4bec-368">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="e4bec-369">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm)：讀取用來執行資料庫作業的伺服器實例。</span><span class="sxs-lookup"><span data-stu-id="e4bec-369">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm): Reads the server instance for performing database operations.</span></span> <span data-ttu-id="e4bec-370">此類別的建構函式是使用 MongoDB 連接字串提供：</span><span class="sxs-lookup"><span data-stu-id="e4bec-370">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="e4bec-371">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm)：代表用於執行作業的 Mongo 資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4bec-371">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm): Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="e4bec-372">本教學課程在介面中使用一般的 [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) 方法來存取特定集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="e4bec-372">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="e4bec-373">呼叫此方法之後，針對集合執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-373">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="e4bec-374">在 `GetCollection<TDocument>(collection)` 方法呼叫中：</span><span class="sxs-lookup"><span data-stu-id="e4bec-374">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="e4bec-375">`collection` 代表集合名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-375">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="e4bec-376">`TDocument` 代表儲存在集合中的 CLR 物件類型。</span><span class="sxs-lookup"><span data-stu-id="e4bec-376">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="e4bec-377">`GetCollection<TDocument>(collection)` 傳回代表集合的 [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) 物件。</span><span class="sxs-lookup"><span data-stu-id="e4bec-377">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="e4bec-378">在此教學課程中，會在集合上叫用下列方法：</span><span class="sxs-lookup"><span data-stu-id="e4bec-378">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="e4bec-379">[Collection.deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm)：刪除符合所提供搜尋條件的單一檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-379">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm): Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="e4bec-380">[尋找 \<TDocument> ](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm)：傳回集合中符合所提供搜尋條件的所有檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-380">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm): Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="e4bec-381">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm)：將提供的物件插入為集合中的新檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-381">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm): Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="e4bec-382">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm)：使用提供的物件取代符合所提供搜尋條件的單一檔。</span><span class="sxs-lookup"><span data-stu-id="e4bec-382">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm): Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="e4bec-383">新增控制器</span><span class="sxs-lookup"><span data-stu-id="e4bec-383">Add a controller</span></span>

<span data-ttu-id="e4bec-384">新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：</span><span class="sxs-lookup"><span data-stu-id="e4bec-384">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="e4bec-385">上述 Web API 控制器會：</span><span class="sxs-lookup"><span data-stu-id="e4bec-385">The preceding web API controller:</span></span>

* <span data-ttu-id="e4bec-386">使用 `BookService` 類別來執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="e4bec-386">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="e4bec-387">包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="e4bec-387">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="e4bec-388">在 `Create` 動作方法中呼叫 <xref:System.Web.Http.ApiController.CreatedAtRoute*>，以傳回 [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 回應。</span><span class="sxs-lookup"><span data-stu-id="e4bec-388">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="e4bec-389">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是狀態碼 201。</span><span class="sxs-lookup"><span data-stu-id="e4bec-389">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="e4bec-390">`CreatedAtRoute` 也會將 `Location` 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="e4bec-390">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="e4bec-391">`Location` 標頭指定新建活頁簿的 URI。</span><span class="sxs-lookup"><span data-stu-id="e4bec-391">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="e4bec-392">測試 Web API</span><span class="sxs-lookup"><span data-stu-id="e4bec-392">Test the web API</span></span>

1. <span data-ttu-id="e4bec-393">建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4bec-393">Build and run the app.</span></span>

1. <span data-ttu-id="e4bec-394">巡覽至 `http://localhost:<port>/api/books`，測試控制器的無參數 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="e4bec-394">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="e4bec-395">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="e4bec-395">The following JSON response is displayed:</span></span>

   ```json
   [
     {
       "id":"5bfd996f7b8e48dc15ff215d",
       "bookName":"Design Patterns",
       "price":54.93,
       "category":"Computers",
       "author":"Ralph Johnson"
     },
     {
       "id":"5bfd996f7b8e48dc15ff215e",
       "bookName":"Clean Code",
       "price":43.15,
       "category":"Computers",
       "author":"Robert C. Martin"
     }
   ]
   ```

1. <span data-ttu-id="e4bec-396">巡覽至 `http://localhost:<port>/api/books/{id here}`，測試控制器的多載 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="e4bec-396">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="e4bec-397">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="e4bec-397">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="e4bec-398">設定 JSON 序列化選項</span><span class="sxs-lookup"><span data-stu-id="e4bec-398">Configure JSON serialization options</span></span>

<span data-ttu-id="e4bec-399">[測試 Web API](#test-the-web-api) 區段中傳回的 JSON 回應，有兩個詳細資訊要變更：</span><span class="sxs-lookup"><span data-stu-id="e4bec-399">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="e4bec-400">屬性名稱的預設駝峰式命名法大小寫應予變更，使其符合CLR 物件屬性名稱的 Pascal 命名法大小寫。</span><span class="sxs-lookup"><span data-stu-id="e4bec-400">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="e4bec-401">`bookName` 屬性應傳回為 `Name`。</span><span class="sxs-lookup"><span data-stu-id="e4bec-401">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="e4bec-402">為滿足上述需求，請進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="e4bec-402">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="e4bec-403">在 `Startup.ConfigureServices` 中，將下列醒目提示的程式碼鏈結至 `AddMvc` 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="e4bec-403">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="e4bec-404">完成前述變更後，Web API 序列化 JSON 回應中屬性名稱即符合其對應的 CLR 物件類型屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-404">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="e4bec-405">例如，`Book` 類別的 `Author` 屬性會序列化為 `Author`。</span><span class="sxs-lookup"><span data-stu-id="e4bec-405">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="e4bec-406">在*模型/Book .cs*中， `BookName` 使用下列屬性標注屬性 [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) ：</span><span class="sxs-lookup"><span data-stu-id="e4bec-406">In *Models/Book.cs*, annotate the `BookName` property with the following [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="e4bec-407">`[JsonProperty]` 的屬性值 `Name` 代表 Web API 序列化 JSON 回應中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="e4bec-407">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="e4bec-408">在 *Models/Book.cs* 的頂端新增下列程式碼，以解析 `[JsonProperty]` 屬性參考：</span><span class="sxs-lookup"><span data-stu-id="e4bec-408">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="e4bec-409">重複[測試 Web API](#test-the-web-api) 一節中定義的步驟。</span><span class="sxs-lookup"><span data-stu-id="e4bec-409">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="e4bec-410">請注意 JSON 屬性名稱中的差異。</span><span class="sxs-lookup"><span data-stu-id="e4bec-410">Notice the difference in JSON property names.</span></span>

::: moniker-end

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="e4bec-411">將驗證支援新增至 Web API</span><span class="sxs-lookup"><span data-stu-id="e4bec-411">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="next-steps"></a><span data-ttu-id="e4bec-412">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e4bec-412">Next steps</span></span>

<span data-ttu-id="e4bec-413">如需有關建置 ASP.NET Core Web API 的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="e4bec-413">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="e4bec-414">這篇文章的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="e4bec-414">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
