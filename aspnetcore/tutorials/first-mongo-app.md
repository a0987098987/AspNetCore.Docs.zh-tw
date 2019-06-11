---
title: 使用 ASP.NET Core 與 MongoDB 建立 Web API
author: prkhandelwal
description: 此教學課程示範如何使用 MongoDB NoSQL 資料庫建立 ASP.NET Core Web API。
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/04/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 6a8c5d75f562b38015101e039a2f5d96a5491595
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692558"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="44717-103">使用 ASP.NET Core 與 MongoDB 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="44717-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="44717-104">作者：[Pratik Khandelwal](https://twitter.com/K2Prk) 與 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="44717-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="44717-105">此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。</span><span class="sxs-lookup"><span data-stu-id="44717-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="44717-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="44717-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="44717-107">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="44717-107">Configure MongoDB</span></span>
> * <span data-ttu-id="44717-108">建立 MongoDB 資料庫</span><span class="sxs-lookup"><span data-stu-id="44717-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="44717-109">定義 MongoDB 集合與結構描述</span><span class="sxs-lookup"><span data-stu-id="44717-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="44717-110">從 Web API 執行 MongoDB CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="44717-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="44717-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="44717-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44717-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="44717-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="44717-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44717-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="44717-114">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="44717-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="44717-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 和 **ASP.NET 與 Web 開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="44717-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="44717-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="44717-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="44717-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44717-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="44717-118">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="44717-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="44717-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44717-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="44717-120">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44717-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="44717-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="44717-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="44717-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="44717-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="44717-123">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="44717-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="44717-124">Visual Studio for Mac 7.7 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="44717-124">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="44717-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="44717-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="44717-126">設定 MongoDB</span><span class="sxs-lookup"><span data-stu-id="44717-126">Configure MongoDB</span></span>

<span data-ttu-id="44717-127">若使用 Windows，MongoDB 預設會安裝在 *C:\\Program Files\\MongoDB*。</span><span class="sxs-lookup"><span data-stu-id="44717-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="44717-128">將 *C:\\Program Files\\MongoDB\\Server\\\<版本號碼>\\bin* 新增到 `Path` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="44717-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="44717-129">此變更會啟用從您開發機器上的任意位置存取 MongoDB 的功能。</span><span class="sxs-lookup"><span data-stu-id="44717-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="44717-130">在下列步驟中使用 mongo 殼層來建立資料庫、建立集合及存放文件。</span><span class="sxs-lookup"><span data-stu-id="44717-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="44717-131">如需有關 mongo 殼層命令的詳細資訊，請參閱[使用 mongo 殼層](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)。</span><span class="sxs-lookup"><span data-stu-id="44717-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="44717-132">選擇您開發機器上的目錄來存放資料。</span><span class="sxs-lookup"><span data-stu-id="44717-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="44717-133">例如， Windows 上的 *C:\\BooksData*。</span><span class="sxs-lookup"><span data-stu-id="44717-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="44717-134">若該目錄不存在，請建立它。</span><span class="sxs-lookup"><span data-stu-id="44717-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="44717-135">mongo 殼層不會建立新目錄。</span><span class="sxs-lookup"><span data-stu-id="44717-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="44717-136">開啟命令殼層。</span><span class="sxs-lookup"><span data-stu-id="44717-136">Open a command shell.</span></span> <span data-ttu-id="44717-137">執行下列命令以連線到預設連接埠 27017 上的 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="44717-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="44717-138">請記得將 `<data_directory_path>` 取代為您在上一個步驟中選擇的目錄。</span><span class="sxs-lookup"><span data-stu-id="44717-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="44717-139">開啟另一個命令殼層執行個體。</span><span class="sxs-lookup"><span data-stu-id="44717-139">Open another command shell instance.</span></span> <span data-ttu-id="44717-140">執行下列命令以連線到預設測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="44717-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="44717-141">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="44717-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="44717-142">若它不存在，會建立名為 *BookstoreDb* 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="44717-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="44717-143">若該資料庫存在，會開啟其連線進行交易。</span><span class="sxs-lookup"><span data-stu-id="44717-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="44717-144">使用下列命令建立 `Books` 集合：</span><span class="sxs-lookup"><span data-stu-id="44717-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="44717-145">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="44717-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="44717-146">使用下列命令為 `Books` 集合定義結構描述並插入兩份文件：</span><span class="sxs-lookup"><span data-stu-id="44717-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="44717-147">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="44717-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="44717-148">使用下列命令檢視資料庫中的文件：</span><span class="sxs-lookup"><span data-stu-id="44717-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="44717-149">會顯示下列結果：</span><span class="sxs-lookup"><span data-stu-id="44717-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="44717-150">結構描述會為每份文件新增自動彙總的 `_id` 屬性 (類型 `ObjectId`)。</span><span class="sxs-lookup"><span data-stu-id="44717-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="44717-151">資料庫已就緒。</span><span class="sxs-lookup"><span data-stu-id="44717-151">The database is ready.</span></span> <span data-ttu-id="44717-152">您可以開始建立 ASP.NET Core Web API。</span><span class="sxs-lookup"><span data-stu-id="44717-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="44717-153">建立 ASP.NET Core Web API 專案</span><span class="sxs-lookup"><span data-stu-id="44717-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="44717-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44717-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="44717-155">移至 [檔案]   > [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="44717-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="44717-156">選取 [ASP.NET Core Web 應用程式]  專案類型，然後選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="44717-156">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="44717-157">將專案命名為 *BooksApi*，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="44717-157">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="44717-158">選取 [.NET Core]  目標架構與 [ASP.NET Core 2.2]  。</span><span class="sxs-lookup"><span data-stu-id="44717-158">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="44717-159">選取 [API]  專案範本，然後選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="44717-159">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="44717-160">造訪 [NuGet 資源庫：MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) 來判斷 MongoDB 的最新穩定 .NET 驅動程式版本。</span><span class="sxs-lookup"><span data-stu-id="44717-160">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="44717-161">在 [套件管理員主控台]  視窗中，瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="44717-161">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="44717-162">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="44717-162">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="44717-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44717-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="44717-164">在命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="44717-164">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="44717-165">會產生以 .NET Core 為目標的新 ASP.NET Core Web API 專案，並在 Visual Studio Code 中開啟。</span><span class="sxs-lookup"><span data-stu-id="44717-165">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="44717-166">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問 **'BooksApi' 中是否遺漏了建置和偵錯的必要資產。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="44717-166">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="44717-167">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="44717-167">Select **Yes**.</span></span>
1. <span data-ttu-id="44717-168">造訪 [NuGet 資源庫：MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) 來判斷 MongoDB 的最新穩定 .NET 驅動程式版本。</span><span class="sxs-lookup"><span data-stu-id="44717-168">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="44717-169">開啟 [整合式終端機]  並瀏覽到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="44717-169">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="44717-170">執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：</span><span class="sxs-lookup"><span data-stu-id="44717-170">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="44717-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="44717-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="44717-172">移至 [檔案]   > [新增解決方案]   > [.NET Core]   > [應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="44717-172">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="44717-173">選取 [ASP.NET Core Web API]  C# 專案範本，然後選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="44717-173">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="44717-174">從 [目標 Framework]  下拉式清單選取 [.NET Core 2.2]  ，然後選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="44717-174">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="44717-175">在 [專案名稱]  中輸入 *BooksApi*，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="44717-175">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="44717-176">在 [方案]  台中，以滑鼠右鍵按一下專案的 [相依性]  節點並選取 [新增封裝]  。</span><span class="sxs-lookup"><span data-stu-id="44717-176">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="44717-177">在搜尋方塊中輸入 *MongoDB.Driver*，然後依序選取 *MongoDB.Driver* 套件和 [新增套件]  。</span><span class="sxs-lookup"><span data-stu-id="44717-177">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="44717-178">選取 [授權接受]  對話方塊中的 [接受]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="44717-178">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="44717-179">新增實體模型</span><span class="sxs-lookup"><span data-stu-id="44717-179">Add an entity model</span></span>

1. <span data-ttu-id="44717-180">新增 *Models* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="44717-180">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="44717-181">新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="44717-181">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

    <span data-ttu-id="44717-182">在上面的類別中，需要 `Id` 屬性：</span><span class="sxs-lookup"><span data-stu-id="44717-182">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="44717-183">才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。</span><span class="sxs-lookup"><span data-stu-id="44717-183">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="44717-184">使用 [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) 加上附註以指定此屬性作為文件的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="44717-184">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="44717-185">使用 [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) 加上附註讓參數傳遞為類型 `string`，而非 [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) 結構。</span><span class="sxs-lookup"><span data-stu-id="44717-185">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="44717-186">Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。</span><span class="sxs-lookup"><span data-stu-id="44717-186">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="44717-187">該類別中的其他屬性是使用 [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) 屬性加上註解。</span><span class="sxs-lookup"><span data-stu-id="44717-187">Other properties in the class are annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="44717-188">屬性的值代表 MongoDB 集合中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="44717-188">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="44717-189">新增組態模型</span><span class="sxs-lookup"><span data-stu-id="44717-189">Add a configuration model</span></span>

1. <span data-ttu-id="44717-190">將下列資料庫組態值新增至 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="44717-190">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="44717-191">使用下列程式碼將 *BookstoreDatabaseSettings.cs* 檔案新增至 *Models* 目錄：</span><span class="sxs-lookup"><span data-stu-id="44717-191">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="44717-192">上述 `BookstoreDatabaseSettings` 類別用來儲存 *appsettings.json* 檔案的 `BookstoreDatabaseSettings` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="44717-192">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="44717-193">JSON 和 C# 屬性名稱以相同方式命名，以簡化對應程序。</span><span class="sxs-lookup"><span data-stu-id="44717-193">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="44717-194">在呼叫 `AddMvc` 之前，將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="44717-194">Add the following code to `Startup.ConfigureServices`, before the call to `AddMvc`:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureDatabaseSettings)]

    <span data-ttu-id="44717-195">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="44717-195">In the preceding code:</span></span>

    * <span data-ttu-id="44717-196">*appsettings.json* 檔案之 `BookstoreDatabaseSettings` 區段所繫結的組態執行個體，是在相依性插入 (DI) 容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="44717-196">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="44717-197">例如，`BookstoreDatabaseSettings` 物件的 `ConnectionString` 屬性會填入 *appsettings.json* 中的 `BookstoreDatabaseSettings:ConnectionString` 屬性。</span><span class="sxs-lookup"><span data-stu-id="44717-197">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="44717-198">`IBookstoreDatabaseSettings` 介面使用 singleton [服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)在 DI 中註冊。</span><span class="sxs-lookup"><span data-stu-id="44717-198">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="44717-199">插入時，介面執行個體會解析成 `BookstoreDatabaseSettings` 物件。</span><span class="sxs-lookup"><span data-stu-id="44717-199">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="44717-200">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookstoreDatabaseSettings` 和 `IBookstoreDatabaseSettings` 參考：</span><span class="sxs-lookup"><span data-stu-id="44717-200">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="44717-201">新增 CRUD 作業服務</span><span class="sxs-lookup"><span data-stu-id="44717-201">Add a CRUD operations service</span></span>

1. <span data-ttu-id="44717-202">新增 *Services* 目錄到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="44717-202">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="44717-203">新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：</span><span class="sxs-lookup"><span data-stu-id="44717-203">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="44717-204">在上述程式碼中，透過建構函式插入從 DI 擷取 `IBookstoreDatabaseSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="44717-204">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="44717-205">這項技術可讓您存取[新增組態模型](#add-a-configuration-model)一節中已新增的 *appsettings.json* 組態值。</span><span class="sxs-lookup"><span data-stu-id="44717-205">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="44717-206">在 `Startup.ConfigureServices` 中，向 DI 註冊 `BookService` 類別：</span><span class="sxs-lookup"><span data-stu-id="44717-206">In `Startup.ConfigureServices`, register the `BookService` class with DI:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=9)]

    <span data-ttu-id="44717-207">在上述程式碼中，`BookService` 類別要向 DI 註冊才能在取用類別中支援建構函式插入。</span><span class="sxs-lookup"><span data-stu-id="44717-207">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="44717-208">因為 `BookService` 直接依存於 `MongoClient`，所以 Singleton 服務存留期最適合。</span><span class="sxs-lookup"><span data-stu-id="44717-208">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="44717-209">依照正式的 [Mongo 用戶端重複使用指導方針](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)，`MongoClient` 應該在 DI 註冊 Singleton 服務存留期。</span><span class="sxs-lookup"><span data-stu-id="44717-209">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="44717-210">在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookService` 參考：</span><span class="sxs-lookup"><span data-stu-id="44717-210">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="44717-211">`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：</span><span class="sxs-lookup"><span data-stu-id="44717-211">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="44717-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; 讀取用於執行資料庫作業的伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="44717-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="44717-213">此類別的建構函式是使用 MongoDB 連接字串提供：</span><span class="sxs-lookup"><span data-stu-id="44717-213">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="44717-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; 代表用於執行作業的 Mongo 資料庫。</span><span class="sxs-lookup"><span data-stu-id="44717-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="44717-215">本教學課程在介面中使用一般的 [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) 方法來存取特定集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="44717-215">This tutorial uses the generic [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="44717-216">呼叫此方法之後，針對集合執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="44717-216">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="44717-217">在 `GetCollection<TDocument>(collection)` 方法呼叫中：</span><span class="sxs-lookup"><span data-stu-id="44717-217">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="44717-218">`collection` 代表集合名稱。</span><span class="sxs-lookup"><span data-stu-id="44717-218">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="44717-219">`TDocument` 代表儲存在集合中的 CLR 物件類型。</span><span class="sxs-lookup"><span data-stu-id="44717-219">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="44717-220">`GetCollection<TDocument>(collection)` 傳回代表集合的 [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) 物件。</span><span class="sxs-lookup"><span data-stu-id="44717-220">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="44717-221">在此教學課程中，會在集合上叫用下列方法：</span><span class="sxs-lookup"><span data-stu-id="44717-221">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="44717-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; 會刪除符合所提供搜尋條件的單一文件。</span><span class="sxs-lookup"><span data-stu-id="44717-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="44717-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; 傳回集合中符合所提供搜尋條件的所有文件。</span><span class="sxs-lookup"><span data-stu-id="44717-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="44717-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; 插入所提供物件作為集合中的新文件。</span><span class="sxs-lookup"><span data-stu-id="44717-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="44717-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; 使用所提供物件取代符合所提供搜尋條件的單一文件。</span><span class="sxs-lookup"><span data-stu-id="44717-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="44717-226">新增控制器</span><span class="sxs-lookup"><span data-stu-id="44717-226">Add a controller</span></span>

<span data-ttu-id="44717-227">新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：</span><span class="sxs-lookup"><span data-stu-id="44717-227">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="44717-228">上述 Web API 控制器會：</span><span class="sxs-lookup"><span data-stu-id="44717-228">The preceding web API controller:</span></span>

* <span data-ttu-id="44717-229">使用 `BookService` 類別來執行 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="44717-229">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="44717-230">包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="44717-230">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="44717-231">在 `Create` 動作方法中呼叫 <xref:System.Web.Http.ApiController.CreatedAtRoute*>，以傳回 [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 回應。</span><span class="sxs-lookup"><span data-stu-id="44717-231">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="44717-232">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是狀態碼 201。</span><span class="sxs-lookup"><span data-stu-id="44717-232">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="44717-233">`CreatedAtRoute` 也會將 `Location` 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="44717-233">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="44717-234">`Location` 標頭指定新建活頁簿的 URI。</span><span class="sxs-lookup"><span data-stu-id="44717-234">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="44717-235">測試 Web API</span><span class="sxs-lookup"><span data-stu-id="44717-235">Test the web API</span></span>

1. <span data-ttu-id="44717-236">建置和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="44717-236">Build and run the app.</span></span>

1. <span data-ttu-id="44717-237">巡覽至 `http://localhost:<port>/api/books`，測試控制器的無參數 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="44717-237">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="44717-238">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="44717-238">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="44717-239">巡覽至 `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e`，測試控制器的多載 `Get` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="44717-239">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="44717-240">會顯示下列 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="44717-240">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="44717-241">後續步驟</span><span class="sxs-lookup"><span data-stu-id="44717-241">Next steps</span></span>

<span data-ttu-id="44717-242">如需有關建置 ASP.NET Core Web API 的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="44717-242">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="44717-243">本文的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="44717-243">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
