---
title: 使用 ASP.NET Core 與 MongoDB 建立 Web API
author: prkhandelwal
description: 此教學課程示範如何使用 MongoDB NoSQL 資料庫建立 ASP.NET Core Web API。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 42c0efcd914eaa54134827cdf3bd6bd599d512b2
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2019
ms.locfileid: "73427016"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="b7064-103">使用 ASP.NET Core 與 MongoDB 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="b7064-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="b7064-104">作者：[Pratik Khandelwal](https://twitter.com/K2Prk) 與 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="b7064-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b7064-105">此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="b7064-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b7064-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7064-107">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-107">Configure MongoDB</span></span>
> * <span data-ttu-id="b7064-108">建立 MongoDB 資料庫</span><span class="sxs-lookup"><span data-stu-id="b7064-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="b7064-109">定義 MongoDB 集合與結構描述</span><span class="sxs-lookup"><span data-stu-id="b7064-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="b7064-110">從 Web API 執行 MongoDB CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="b7064-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="b7064-111">自訂 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="b7064-111">Customize JSON serialization</span></span>

<span data-ttu-id="b7064-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b7064-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7064-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="b7064-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7064-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7064-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="b7064-115">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="b7064-115">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="b7064-116">隨附 **ASP.NET 和網頁程式開發**工作負載的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="b7064-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="b7064-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7064-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="b7064-119">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="b7064-119">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b7064-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="b7064-121">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="b7064-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7064-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7064-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="b7064-124">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="b7064-124">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b7064-125">Visual Studio for Mac 7.7 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="b7064-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="b7064-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="b7064-127">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-127">Configure MongoDB</span></span>

<span data-ttu-id="b7064-128">若使用 Windows，MongoDB 預設會安裝在 *C:\\Program Files\\MongoDB*。</span><span class="sxs-lookup"><span data-stu-id="b7064-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="b7064-129">將 *C:\\Program Files\\MongoDB\\Server\\\<版本號碼>\\bin* 新增到 `Path` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="b7064-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="b7064-130">此變更會啟用從您開發機器上的任意位置存取 MongoDB 的功能。</span><span class="sxs-lookup"><span data-stu-id="b7064-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="b7064-131">在下列步驟中使用 mongo 殼層來建立資料庫、建立集合及存放文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="b7064-132">如需有關 mongo 殼層命令的詳細資訊，請參閱[使用 mongo 殼層](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)。</span><span class="sxs-lookup"><span data-stu-id="b7064-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="b7064-133">選擇您開發機器上的目錄來存放資料。</span><span class="sxs-lookup"><span data-stu-id="b7064-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="b7064-134">例如， Windows 上的 *C:\\BooksData*。</span><span class="sxs-lookup"><span data-stu-id="b7064-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="b7064-135">若該目錄不存在，請建立它。</span><span class="sxs-lookup"><span data-stu-id="b7064-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="b7064-136">mongo 殼層不會建立新目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="b7064-137">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="b7064-137">Open a command shell.</span></span> <span data-ttu-id="b7064-138">執行下列命令以連線到預設連接埠 27017 上的 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="b7064-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="b7064-139">請記得將 `<data_directory_path>` 取代為您在上一個步驟中選擇的目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="b7064-140">開啟另一個命令殼層執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7064-140">Open another command shell instance.</span></span> <span data-ttu-id="b7064-141">執行下列命令以連線到預設測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="b7064-141">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="b7064-142">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7064-142">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="b7064-143">若它不存在，會建立名為 *BookstoreDb* 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7064-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="b7064-144">若該資料庫存在，會開啟其連線進行交易。</span><span class="sxs-lookup"><span data-stu-id="b7064-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="b7064-145">使用下列命令建立 `Books` 集合：</span><span class="sxs-lookup"><span data-stu-id="b7064-145">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="b7064-146">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="b7064-146">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="b7064-147">使用下列命令為 `Books` 集合定義結構描述並插入兩份文件：</span><span class="sxs-lookup"><span data-stu-id="b7064-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="b7064-148">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="b7064-148">The following result is displayed:</span></span>

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
   > <span data-ttu-id="b7064-149">您執行此範例時的識別碼，將不同於本文中顯示的識別碼。</span><span class="sxs-lookup"><span data-stu-id="b7064-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="b7064-150">使用下列命令檢視資料庫中的文件：</span><span class="sxs-lookup"><span data-stu-id="b7064-150">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="b7064-151">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="b7064-151">The following result is displayed:</span></span>

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

   <span data-ttu-id="b7064-152">結構描述會為每份文件新增自動彙總的 `_id` 屬性 (類型 `ObjectId`)。</span><span class="sxs-lookup"><span data-stu-id="b7064-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="b7064-153">資料庫已就緒。</span><span class="sxs-lookup"><span data-stu-id="b7064-153">The database is ready.</span></span> <span data-ttu-id="b7064-154">您可以開始建立 ASP.NET Core Web API。</span><span class="sxs-lookup"><span data-stu-id="b7064-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="b7064-155">建立 ASP.NET Core Web API 專案</span><span class="sxs-lookup"><span data-stu-id="b7064-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7064-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7064-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b7064-157">前往 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="b7064-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="b7064-158">選取 [ASP.NET Core Web 應用程式] 專案類型，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7064-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="b7064-159">將專案命名為 *BooksApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="b7064-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="b7064-160">選取 [.NET Core] 目標架構與 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="b7064-160">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="b7064-161">選取 [API] 專案範本，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b7064-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="b7064-162">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="b7064-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="b7064-163">在 [套件管理員主控台] 視窗中，瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="b7064-164">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="b7064-164">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7064-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="b7064-166">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7064-166">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="b7064-167">會產生以 .NET Core 為目標的新 ASP.NET Core Web API 專案，並在 Visual Studio Code 中開啟。</span><span class="sxs-lookup"><span data-stu-id="b7064-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="b7064-168">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' BooksApi ' 中遺漏了 debug。加入它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="b7064-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="b7064-169">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="b7064-169">Select **Yes**.</span></span>
1. <span data-ttu-id="b7064-170">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="b7064-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="b7064-171">開啟 [整合式終端機] 並瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="b7064-172">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="b7064-172">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7064-173">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7064-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="b7064-174">前往 [檔案] > [新增解決方案] > [.NET Core] > [應用程式]。</span><span class="sxs-lookup"><span data-stu-id="b7064-174">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="b7064-175">選取 [ASP.NET Core Web API] C# 專案範本，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7064-175">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="b7064-176">從 [目標 Framework] 下拉式清單選取 [.NET Core 3.0]，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7064-176">Select **.NET Core 3.0** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="b7064-177">在 [專案名稱] 中輸入 *BooksApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="b7064-177">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="b7064-178">在 [方案] 台中，以滑鼠右鍵按一下專案的 [相依性] 節點並選取 [新增封裝]。</span><span class="sxs-lookup"><span data-stu-id="b7064-178">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="b7064-179">在搜尋方塊中輸入 *MongoDB.Driver*，然後依序選取 *MongoDB.Driver* 套件和 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="b7064-179">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="b7064-180">選取 [授權接受] 對話方塊中的 [接受] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b7064-180">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="b7064-181">新增實體模型</span><span class="sxs-lookup"><span data-stu-id="b7064-181">Add an entity model</span></span>

1. <span data-ttu-id="b7064-182">新增 *Models* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-182">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="b7064-183">新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-183">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

   <span data-ttu-id="b7064-184">在上面的類別中，需要 `Id` 屬性：</span><span class="sxs-lookup"><span data-stu-id="b7064-184">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="b7064-185">才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。</span><span class="sxs-lookup"><span data-stu-id="b7064-185">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="b7064-186">使用 [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) 加上附註以指定此屬性作為文件的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b7064-186">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="b7064-187">使用 [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) 加上附註讓參數傳遞為類型 `string`，而非 [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) 結構。</span><span class="sxs-lookup"><span data-stu-id="b7064-187">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="b7064-188">Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-188">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="b7064-189">使用 [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) 屬性標註 `BookName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b7064-189">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="b7064-190">`Name` 的屬性值代表 MongoDB 集合中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-190">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="b7064-191">新增組態模型</span><span class="sxs-lookup"><span data-stu-id="b7064-191">Add a configuration model</span></span>

1. <span data-ttu-id="b7064-192">將下列資料庫組態值新增至 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="b7064-192">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="b7064-193">使用下列程式碼將 *BookstoreDatabaseSettings.cs* 檔案新增至 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-193">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="b7064-194">上述 `BookstoreDatabaseSettings` 類別用來儲存 *appsettings.json* 檔案的 `BookstoreDatabaseSettings` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="b7064-194">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="b7064-195">JSON 和 C# 屬性名稱以相同方式命名，以簡化對應程序。</span><span class="sxs-lookup"><span data-stu-id="b7064-195">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="b7064-196">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="b7064-196">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="b7064-197">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="b7064-197">In the preceding code:</span></span>

   * <span data-ttu-id="b7064-198">*appsettings.json* 檔案之 `BookstoreDatabaseSettings` 區段所繫結的組態執行個體，是在相依性插入 (DI) 容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="b7064-198">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="b7064-199">例如，`BookstoreDatabaseSettings` 物件的 `ConnectionString` 屬性會填入 *appsettings.json* 中的 `BookstoreDatabaseSettings:ConnectionString` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b7064-199">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="b7064-200">`IBookstoreDatabaseSettings` 介面使用 singleton [服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)在 DI 中註冊。</span><span class="sxs-lookup"><span data-stu-id="b7064-200">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="b7064-201">插入時，介面執行個體會解析成 `BookstoreDatabaseSettings` 物件。</span><span class="sxs-lookup"><span data-stu-id="b7064-201">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="b7064-202">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookstoreDatabaseSettings` 和 `IBookstoreDatabaseSettings` 參考：</span><span class="sxs-lookup"><span data-stu-id="b7064-202">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="b7064-203">新增 CRUD 作業服務</span><span class="sxs-lookup"><span data-stu-id="b7064-203">Add a CRUD operations service</span></span>

1. <span data-ttu-id="b7064-204">新增 *Services* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-204">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="b7064-205">新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-205">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="b7064-206">在上述程式碼中，透過建構函式插入從 DI 擷取 `IBookstoreDatabaseSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7064-206">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="b7064-207">這項技術可讓您存取[新增組態模型](#add-a-configuration-model)一節中已新增的 *appsettings.json* 組態值。</span><span class="sxs-lookup"><span data-stu-id="b7064-207">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="b7064-208">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="b7064-208">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="b7064-209">在上述程式碼中，`BookService` 類別要向 DI 註冊才能在取用類別中支援建構函式插入。</span><span class="sxs-lookup"><span data-stu-id="b7064-209">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="b7064-210">因為 `BookService` 直接依存於 `MongoClient`，所以 Singleton 服務存留期最適合。</span><span class="sxs-lookup"><span data-stu-id="b7064-210">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="b7064-211">依照正式的 [Mongo 用戶端重複使用指導方針](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)，`MongoClient` 應該在 DI 註冊 Singleton 服務存留期。</span><span class="sxs-lookup"><span data-stu-id="b7064-211">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="b7064-212">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookService` 參考：</span><span class="sxs-lookup"><span data-stu-id="b7064-212">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="b7064-213">`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：</span><span class="sxs-lookup"><span data-stu-id="b7064-213">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="b7064-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; 讀取用於執行資料庫作業的伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7064-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="b7064-215">此類別的建構函式是使用 MongoDB 連接字串提供：</span><span class="sxs-lookup"><span data-stu-id="b7064-215">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="b7064-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; 代表用於執行作業的 Mongo 資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7064-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="b7064-217">本教學課程在介面上使用一般的 [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) 方法來存取特定集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="b7064-217">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="b7064-218">呼叫此方法之後，針對集合執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-218">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="b7064-219">在 `GetCollection<TDocument>(collection)` 方法呼叫中：</span><span class="sxs-lookup"><span data-stu-id="b7064-219">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="b7064-220">`collection` 代表集合名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-220">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="b7064-221">`TDocument` 代表儲存在集合中的 CLR 物件類型。</span><span class="sxs-lookup"><span data-stu-id="b7064-221">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="b7064-222">`GetCollection<TDocument>(collection)` 傳回代表集合的 [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) 物件。</span><span class="sxs-lookup"><span data-stu-id="b7064-222">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="b7064-223">在此教學課程中，會在集合上叫用下列方法：</span><span class="sxs-lookup"><span data-stu-id="b7064-223">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="b7064-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; 會刪除符合所提供搜尋條件的單一文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="b7064-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; 傳回集合中符合所提供搜尋條件的所有文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="b7064-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; 插入所提供物件作為集合中的新文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="b7064-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; 使用所提供物件取代符合所提供搜尋條件的單一文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="b7064-228">新增控制器</span><span class="sxs-lookup"><span data-stu-id="b7064-228">Add a controller</span></span>

<span data-ttu-id="b7064-229">新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-229">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="b7064-230">上述 Web API 控制器會：</span><span class="sxs-lookup"><span data-stu-id="b7064-230">The preceding web API controller:</span></span>

* <span data-ttu-id="b7064-231">使用 `BookService` 類別來執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-231">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="b7064-232">包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="b7064-232">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="b7064-233">在 `Create` 動作方法中呼叫 <xref:System.Web.Http.ApiController.CreatedAtRoute*>，以傳回 [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 回應。</span><span class="sxs-lookup"><span data-stu-id="b7064-233">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="b7064-234">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是狀態碼 201。</span><span class="sxs-lookup"><span data-stu-id="b7064-234">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="b7064-235">`CreatedAtRoute` 也會將 `Location` 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="b7064-235">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="b7064-236">`Location` 標頭指定新建活頁簿的 URI。</span><span class="sxs-lookup"><span data-stu-id="b7064-236">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="b7064-237">測試 Web API</span><span class="sxs-lookup"><span data-stu-id="b7064-237">Test the web API</span></span>

1. <span data-ttu-id="b7064-238">建置和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7064-238">Build and run the app.</span></span>

1. <span data-ttu-id="b7064-239">巡覽至 `http://localhost:<port>/api/books`，測試控制器的無參數 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="b7064-239">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="b7064-240">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="b7064-240">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="b7064-241">巡覽至 `http://localhost:<port>/api/books/{id here}`，測試控制器的多載 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="b7064-241">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="b7064-242">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="b7064-242">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="b7064-243">設定 JSON 序列化選項</span><span class="sxs-lookup"><span data-stu-id="b7064-243">Configure JSON serialization options</span></span>

<span data-ttu-id="b7064-244">[測試 Web API](#test-the-web-api) 區段中傳回的 JSON 回應，有兩個詳細資訊要變更：</span><span class="sxs-lookup"><span data-stu-id="b7064-244">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="b7064-245">屬性名稱的預設駝峰式命名法大小寫應予變更，使其符合CLR 物件屬性名稱的 Pascal 命名法大小寫。</span><span class="sxs-lookup"><span data-stu-id="b7064-245">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="b7064-246">`bookName` 屬性應傳回為 `Name`。</span><span class="sxs-lookup"><span data-stu-id="b7064-246">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="b7064-247">為滿足上述需求，請進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="b7064-247">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="b7064-248">已從 ASP.NET 共用架構移除 JSON.NET。</span><span class="sxs-lookup"><span data-stu-id="b7064-248">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="b7064-249">將套件參考新增至 [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson)。</span><span class="sxs-lookup"><span data-stu-id="b7064-249">Add a package reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="b7064-250">在 `Startup.ConfigureServices` 中，將下列醒目提示的程式碼鏈結至 `AddMvc` 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="b7064-250">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="b7064-251">完成前述變更後，Web API 序列化 JSON 回應中屬性名稱即符合其對應的 CLR 物件類型屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-251">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="b7064-252">例如，`Book` 類別的 `Author` 屬性會序列化為 `Author`。</span><span class="sxs-lookup"><span data-stu-id="b7064-252">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="b7064-253">在 *Models/Book.cs* 中，以下列 [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) 屬性標註 `BookName` 屬性：</span><span class="sxs-lookup"><span data-stu-id="b7064-253">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="b7064-254">`[JsonProperty]` 的屬性值 `Name` 代表 Web API 序列化 JSON 回應中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-254">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="b7064-255">在 *Models/Book.cs* 的頂端新增下列程式碼，以解析 `[JsonProperty]` 屬性參考：</span><span class="sxs-lookup"><span data-stu-id="b7064-255">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="b7064-256">重複[測試 Web API](#test-the-web-api) 一節中定義的步驟。</span><span class="sxs-lookup"><span data-stu-id="b7064-256">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="b7064-257">請注意 JSON 屬性名稱中的差異。</span><span class="sxs-lookup"><span data-stu-id="b7064-257">Notice the difference in JSON property names.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b7064-258">此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-258">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="b7064-259">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b7064-259">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7064-260">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-260">Configure MongoDB</span></span>
> * <span data-ttu-id="b7064-261">建立 MongoDB 資料庫</span><span class="sxs-lookup"><span data-stu-id="b7064-261">Create a MongoDB database</span></span>
> * <span data-ttu-id="b7064-262">定義 MongoDB 集合與結構描述</span><span class="sxs-lookup"><span data-stu-id="b7064-262">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="b7064-263">從 Web API 執行 MongoDB CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="b7064-263">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="b7064-264">自訂 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="b7064-264">Customize JSON serialization</span></span>

<span data-ttu-id="b7064-265">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b7064-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7064-266">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="b7064-266">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7064-267">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7064-267">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="b7064-268">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="b7064-268">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="b7064-269">隨附 **ASP.NET 和網頁程式開發**工作負載的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="b7064-269">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="b7064-270">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-270">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7064-271">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-271">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="b7064-272">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="b7064-272">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b7064-273">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-273">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="b7064-274">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-274">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="b7064-275">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-275">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7064-276">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7064-276">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="b7064-277">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="b7064-277">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b7064-278">Visual Studio for Mac 7.7 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="b7064-278">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="b7064-279">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-279">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="b7064-280">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7064-280">Configure MongoDB</span></span>

<span data-ttu-id="b7064-281">若使用 Windows，MongoDB 預設會安裝在 *C:\\Program Files\\MongoDB*。</span><span class="sxs-lookup"><span data-stu-id="b7064-281">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="b7064-282">將 *C:\\Program Files\\MongoDB\\Server\\\<版本號碼>\\bin* 新增到 `Path` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="b7064-282">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="b7064-283">此變更會啟用從您開發機器上的任意位置存取 MongoDB 的功能。</span><span class="sxs-lookup"><span data-stu-id="b7064-283">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="b7064-284">在下列步驟中使用 mongo 殼層來建立資料庫、建立集合及存放文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-284">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="b7064-285">如需有關 mongo 殼層命令的詳細資訊，請參閱[使用 mongo 殼層](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)。</span><span class="sxs-lookup"><span data-stu-id="b7064-285">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="b7064-286">選擇您開發機器上的目錄來存放資料。</span><span class="sxs-lookup"><span data-stu-id="b7064-286">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="b7064-287">例如， Windows 上的 *C:\\BooksData*。</span><span class="sxs-lookup"><span data-stu-id="b7064-287">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="b7064-288">若該目錄不存在，請建立它。</span><span class="sxs-lookup"><span data-stu-id="b7064-288">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="b7064-289">mongo 殼層不會建立新目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-289">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="b7064-290">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="b7064-290">Open a command shell.</span></span> <span data-ttu-id="b7064-291">執行下列命令以連線到預設連接埠 27017 上的 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="b7064-291">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="b7064-292">請記得將 `<data_directory_path>` 取代為您在上一個步驟中選擇的目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-292">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="b7064-293">開啟另一個命令殼層執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7064-293">Open another command shell instance.</span></span> <span data-ttu-id="b7064-294">執行下列命令以連線到預設測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="b7064-294">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="b7064-295">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7064-295">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="b7064-296">若它不存在，會建立名為 *BookstoreDb* 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7064-296">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="b7064-297">若該資料庫存在，會開啟其連線進行交易。</span><span class="sxs-lookup"><span data-stu-id="b7064-297">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="b7064-298">使用下列命令建立 `Books` 集合：</span><span class="sxs-lookup"><span data-stu-id="b7064-298">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="b7064-299">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="b7064-299">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="b7064-300">使用下列命令為 `Books` 集合定義結構描述並插入兩份文件：</span><span class="sxs-lookup"><span data-stu-id="b7064-300">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="b7064-301">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="b7064-301">The following result is displayed:</span></span>

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
   > <span data-ttu-id="b7064-302">您執行此範例時的識別碼，將不同於本文中顯示的識別碼。</span><span class="sxs-lookup"><span data-stu-id="b7064-302">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="b7064-303">使用下列命令檢視資料庫中的文件：</span><span class="sxs-lookup"><span data-stu-id="b7064-303">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="b7064-304">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="b7064-304">The following result is displayed:</span></span>

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

   <span data-ttu-id="b7064-305">結構描述會為每份文件新增自動彙總的 `_id` 屬性 (類型 `ObjectId`)。</span><span class="sxs-lookup"><span data-stu-id="b7064-305">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="b7064-306">資料庫已就緒。</span><span class="sxs-lookup"><span data-stu-id="b7064-306">The database is ready.</span></span> <span data-ttu-id="b7064-307">您可以開始建立 ASP.NET Core Web API。</span><span class="sxs-lookup"><span data-stu-id="b7064-307">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="b7064-308">建立 ASP.NET Core Web API 專案</span><span class="sxs-lookup"><span data-stu-id="b7064-308">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7064-309">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7064-309">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b7064-310">前往 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="b7064-310">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="b7064-311">選取 [ASP.NET Core Web 應用程式] 專案類型，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7064-311">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="b7064-312">將專案命名為 *BooksApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="b7064-312">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="b7064-313">選取 [.NET Core] 目標架構與 [ASP.NET Core 2.2]。</span><span class="sxs-lookup"><span data-stu-id="b7064-313">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="b7064-314">選取 [API] 專案範本，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b7064-314">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="b7064-315">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="b7064-315">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="b7064-316">在 [套件管理員主控台] 視窗中，瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-316">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="b7064-317">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="b7064-317">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7064-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7064-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="b7064-319">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7064-319">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="b7064-320">會產生以 .NET Core 為目標的新 ASP.NET Core Web API 專案，並在 Visual Studio Code 中開啟。</span><span class="sxs-lookup"><span data-stu-id="b7064-320">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="b7064-321">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' BooksApi ' 中遺漏了 debug。加入它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="b7064-321">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="b7064-322">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="b7064-322">Select **Yes**.</span></span>
1. <span data-ttu-id="b7064-323">請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。</span><span class="sxs-lookup"><span data-stu-id="b7064-323">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="b7064-324">開啟 [整合式終端機] 並瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-324">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="b7064-325">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="b7064-325">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7064-326">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7064-326">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="b7064-327">前往 [檔案] > [新增解決方案] > [.NET Core] > [應用程式]。</span><span class="sxs-lookup"><span data-stu-id="b7064-327">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="b7064-328">選取 [ASP.NET Core Web API] C# 專案範本，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7064-328">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="b7064-329">從 [目標 Framework] 下拉式清單選取 [.NET Core 2.2]，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7064-329">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="b7064-330">在 [專案名稱] 中輸入 *BooksApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="b7064-330">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="b7064-331">在 [方案] 台中，以滑鼠右鍵按一下專案的 [相依性] 節點並選取 [新增封裝]。</span><span class="sxs-lookup"><span data-stu-id="b7064-331">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="b7064-332">在搜尋方塊中輸入 *MongoDB.Driver*，然後依序選取 *MongoDB.Driver* 套件和 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="b7064-332">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="b7064-333">選取 [授權接受] 對話方塊中的 [接受] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b7064-333">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="b7064-334">新增實體模型</span><span class="sxs-lookup"><span data-stu-id="b7064-334">Add an entity model</span></span>

1. <span data-ttu-id="b7064-335">新增 *Models* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-335">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="b7064-336">新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-336">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

   <span data-ttu-id="b7064-337">在上面的類別中，需要 `Id` 屬性：</span><span class="sxs-lookup"><span data-stu-id="b7064-337">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="b7064-338">才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。</span><span class="sxs-lookup"><span data-stu-id="b7064-338">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="b7064-339">使用 [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) 加上附註以指定此屬性作為文件的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b7064-339">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="b7064-340">使用 [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) 加上附註讓參數傳遞為類型 `string`，而非 [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) 結構。</span><span class="sxs-lookup"><span data-stu-id="b7064-340">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="b7064-341">Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-341">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="b7064-342">使用 [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) 屬性標註 `BookName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b7064-342">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="b7064-343">`Name` 的屬性值代表 MongoDB 集合中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-343">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="b7064-344">新增組態模型</span><span class="sxs-lookup"><span data-stu-id="b7064-344">Add a configuration model</span></span>

1. <span data-ttu-id="b7064-345">將下列資料庫組態值新增至 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="b7064-345">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="b7064-346">使用下列程式碼將 *BookstoreDatabaseSettings.cs* 檔案新增至 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-346">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="b7064-347">上述 `BookstoreDatabaseSettings` 類別用來儲存 *appsettings.json* 檔案的 `BookstoreDatabaseSettings` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="b7064-347">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="b7064-348">JSON 和 C# 屬性名稱以相同方式命名，以簡化對應程序。</span><span class="sxs-lookup"><span data-stu-id="b7064-348">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="b7064-349">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="b7064-349">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="b7064-350">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="b7064-350">In the preceding code:</span></span>

   * <span data-ttu-id="b7064-351">*appsettings.json* 檔案之 `BookstoreDatabaseSettings` 區段所繫結的組態執行個體，是在相依性插入 (DI) 容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="b7064-351">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="b7064-352">例如，`BookstoreDatabaseSettings` 物件的 `ConnectionString` 屬性會填入 *appsettings.json* 中的 `BookstoreDatabaseSettings:ConnectionString` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b7064-352">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="b7064-353">`IBookstoreDatabaseSettings` 介面使用 singleton [服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)在 DI 中註冊。</span><span class="sxs-lookup"><span data-stu-id="b7064-353">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="b7064-354">插入時，介面執行個體會解析成 `BookstoreDatabaseSettings` 物件。</span><span class="sxs-lookup"><span data-stu-id="b7064-354">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="b7064-355">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookstoreDatabaseSettings` 和 `IBookstoreDatabaseSettings` 參考：</span><span class="sxs-lookup"><span data-stu-id="b7064-355">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="b7064-356">新增 CRUD 作業服務</span><span class="sxs-lookup"><span data-stu-id="b7064-356">Add a CRUD operations service</span></span>

1. <span data-ttu-id="b7064-357">新增 *Services* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b7064-357">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="b7064-358">新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-358">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="b7064-359">在上述程式碼中，透過建構函式插入從 DI 擷取 `IBookstoreDatabaseSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7064-359">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="b7064-360">這項技術可讓您存取[新增組態模型](#add-a-configuration-model)一節中已新增的 *appsettings.json* 組態值。</span><span class="sxs-lookup"><span data-stu-id="b7064-360">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="b7064-361">將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="b7064-361">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="b7064-362">在上述程式碼中，`BookService` 類別要向 DI 註冊才能在取用類別中支援建構函式插入。</span><span class="sxs-lookup"><span data-stu-id="b7064-362">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="b7064-363">因為 `BookService` 直接依存於 `MongoClient`，所以 Singleton 服務存留期最適合。</span><span class="sxs-lookup"><span data-stu-id="b7064-363">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="b7064-364">依照正式的 [Mongo 用戶端重複使用指導方針](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)，`MongoClient` 應該在 DI 註冊 Singleton 服務存留期。</span><span class="sxs-lookup"><span data-stu-id="b7064-364">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="b7064-365">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookService` 參考：</span><span class="sxs-lookup"><span data-stu-id="b7064-365">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="b7064-366">`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：</span><span class="sxs-lookup"><span data-stu-id="b7064-366">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="b7064-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; 讀取用於執行資料庫作業的伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7064-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="b7064-368">此類別的建構函式是使用 MongoDB 連接字串提供：</span><span class="sxs-lookup"><span data-stu-id="b7064-368">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="b7064-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; 代表用於執行作業的 Mongo 資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7064-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="b7064-370">本教學課程在介面上使用一般的 [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) 方法來存取特定集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="b7064-370">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="b7064-371">呼叫此方法之後，針對集合執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-371">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="b7064-372">在 `GetCollection<TDocument>(collection)` 方法呼叫中：</span><span class="sxs-lookup"><span data-stu-id="b7064-372">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="b7064-373">`collection` 代表集合名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-373">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="b7064-374">`TDocument` 代表儲存在集合中的 CLR 物件類型。</span><span class="sxs-lookup"><span data-stu-id="b7064-374">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="b7064-375">`GetCollection<TDocument>(collection)` 傳回代表集合的 [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) 物件。</span><span class="sxs-lookup"><span data-stu-id="b7064-375">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="b7064-376">在此教學課程中，會在集合上叫用下列方法：</span><span class="sxs-lookup"><span data-stu-id="b7064-376">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="b7064-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; 會刪除符合所提供搜尋條件的單一文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="b7064-378">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; 傳回集合中符合所提供搜尋條件的所有文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-378">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="b7064-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; 插入所提供物件作為集合中的新文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="b7064-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; 使用所提供物件取代符合所提供搜尋條件的單一文件。</span><span class="sxs-lookup"><span data-stu-id="b7064-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="b7064-381">新增控制器</span><span class="sxs-lookup"><span data-stu-id="b7064-381">Add a controller</span></span>

<span data-ttu-id="b7064-382">新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：</span><span class="sxs-lookup"><span data-stu-id="b7064-382">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="b7064-383">上述 Web API 控制器會：</span><span class="sxs-lookup"><span data-stu-id="b7064-383">The preceding web API controller:</span></span>

* <span data-ttu-id="b7064-384">使用 `BookService` 類別來執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="b7064-384">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="b7064-385">包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="b7064-385">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="b7064-386">在 `Create` 動作方法中呼叫 <xref:System.Web.Http.ApiController.CreatedAtRoute*>，以傳回 [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 回應。</span><span class="sxs-lookup"><span data-stu-id="b7064-386">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="b7064-387">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是狀態碼 201。</span><span class="sxs-lookup"><span data-stu-id="b7064-387">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="b7064-388">`CreatedAtRoute` 也會將 `Location` 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="b7064-388">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="b7064-389">`Location` 標頭指定新建活頁簿的 URI。</span><span class="sxs-lookup"><span data-stu-id="b7064-389">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="b7064-390">測試 Web API</span><span class="sxs-lookup"><span data-stu-id="b7064-390">Test the web API</span></span>

1. <span data-ttu-id="b7064-391">建置和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7064-391">Build and run the app.</span></span>

1. <span data-ttu-id="b7064-392">巡覽至 `http://localhost:<port>/api/books`，測試控制器的無參數 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="b7064-392">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="b7064-393">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="b7064-393">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="b7064-394">巡覽至 `http://localhost:<port>/api/books/{id here}`，測試控制器的多載 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="b7064-394">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="b7064-395">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="b7064-395">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="b7064-396">設定 JSON 序列化選項</span><span class="sxs-lookup"><span data-stu-id="b7064-396">Configure JSON serialization options</span></span>

<span data-ttu-id="b7064-397">[測試 Web API](#test-the-web-api) 區段中傳回的 JSON 回應，有兩個詳細資訊要變更：</span><span class="sxs-lookup"><span data-stu-id="b7064-397">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="b7064-398">屬性名稱的預設駝峰式命名法大小寫應予變更，使其符合CLR 物件屬性名稱的 Pascal 命名法大小寫。</span><span class="sxs-lookup"><span data-stu-id="b7064-398">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="b7064-399">`bookName` 屬性應傳回為 `Name`。</span><span class="sxs-lookup"><span data-stu-id="b7064-399">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="b7064-400">為滿足上述需求，請進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="b7064-400">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="b7064-401">在 `Startup.ConfigureServices` 中，將下列醒目提示的程式碼鏈結至 `AddMvc` 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="b7064-401">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="b7064-402">完成前述變更後，Web API 序列化 JSON 回應中屬性名稱即符合其對應的 CLR 物件類型屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-402">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="b7064-403">例如，`Book` 類別的 `Author` 屬性會序列化為 `Author`。</span><span class="sxs-lookup"><span data-stu-id="b7064-403">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="b7064-404">在 *Models/Book.cs* 中，以下列 [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) 屬性標註 `BookName` 屬性：</span><span class="sxs-lookup"><span data-stu-id="b7064-404">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="b7064-405">`[JsonProperty]` 的屬性值 `Name` 代表 Web API 序列化 JSON 回應中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="b7064-405">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="b7064-406">在 *Models/Book.cs* 的頂端新增下列程式碼，以解析 `[JsonProperty]` 屬性參考：</span><span class="sxs-lookup"><span data-stu-id="b7064-406">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="b7064-407">重複[測試 Web API](#test-the-web-api) 一節中定義的步驟。</span><span class="sxs-lookup"><span data-stu-id="b7064-407">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="b7064-408">請注意 JSON 屬性名稱中的差異。</span><span class="sxs-lookup"><span data-stu-id="b7064-408">Notice the difference in JSON property names.</span></span>

::: moniker-end

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="b7064-409">將驗證支援新增至 Web API</span><span class="sxs-lookup"><span data-stu-id="b7064-409">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="next-steps"></a><span data-ttu-id="b7064-410">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b7064-410">Next steps</span></span>

<span data-ttu-id="b7064-411">如需有關建置 ASP.NET Core Web API 的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="b7064-411">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="b7064-412">本文的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="b7064-412">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
