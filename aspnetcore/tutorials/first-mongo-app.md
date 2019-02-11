---
title: 使用 ASP.NET Core 與 MongoDB 建置 Web API
author: prkhandelwal
description: 此教學課程示範如何使用 MongoDB NoSQL 資料庫建置 ASP.NET Core Web API。
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 5e146261fdc8354fc9f4295a8af317e5cc36332f
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667332"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>使用 ASP.NET Core 與 MongoDB 建立 Web API

作者：[Pratik Khandelwal](https://twitter.com/K2Prk) 與 [Scott Addie](https://twitter.com/Scott_Addie)

此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 設定 MongoDB
> * 建立 MongoDB 資料庫
> * 定義 MongoDB 集合與結構描述
> * 從 Web API 執行 MongoDB CRUD 作業

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.2 或更新版本](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017 15.9 版或更新版本](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)，包含 **ASP.NET 與網頁程式開發**工作負載
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.2 或更新版本](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [.NET Core SDK 2.2 或更新版本](https://www.microsoft.com/net/download/all)
* [Visual Studio for Mac 7.7 版或更新版本](https://www.visualstudio.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>設定 MongoDB

若使用 Windows，MongoDB 預設會安裝在 *C:\\Program Files\\MongoDB*。 將 *C:\\Program Files\\MongoDB\\Server\\\<版本號碼>\\bin* 新增到 `Path` 環境變數。 此變更會啟用從您開發機器上的任意位置存取 MongoDB 的功能。

在下列步驟中使用 mongo 殼層來建立資料庫、建立集合及存放文件。 如需有關 mongo 殼層命令的詳細資訊，請參閱[使用 mongo 殼層](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)。

1. 選擇您開發機器上的目錄來存放資料。 例如， Windows 上的 *C:\\BooksData*。 若該目錄不存在，請建立它。 mongo 殼層不會建立新目錄。
1. 開啟命令殼層。 執行下列命令以連線到預設連接埠 27017 上的 MongoDB。 請記得將 `<data_directory_path>` 取代為您在上一個步驟中選擇的目錄。

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. 開啟另一個命令殼層執行個體。 執行下列命令以連線到預設測試資料庫：

    ```console
    mongo
    ```

1. 在命令殼層中執行下列命令：

    ```console
    use BookstoreDb
    ```

    若它不存在，會建立名為 *BookstoreDb* 的資料庫。 若該資料庫存在，會開啟其連線進行交易。

1. 使用下列命令建立 `Books` 集合：

    ```console
    db.createCollection('Books')
    ```

    會顯示下列結果：

    ```console
    { "ok" : 1 }
    ```

1. 使用下列命令為 `Books` 集合定義結構描述並插入兩份文件：

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    會顯示下列結果：

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. 使用下列命令檢視資料庫中的文件：

    ```console
    db.Books.find({}).pretty()
    ```

    會顯示下列結果：

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

    結構描述會為每份文件新增自動彙總的 `_id` 屬性 (類型 `ObjectId`)。

資料庫已就緒。 您可以開始建立 ASP.NET Core Web API。

## <a name="create-the-aspnet-core-web-api-project"></a>建立 ASP.NET Core Web API 專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 移至 [檔案] > [新增] > [專案]。
1. 選取 [ASP.NET Core Web 應用程式]、將專案命名為 *BooksApi*，然後按一下 [確定]。
1. 選取 [.NET Core] 目標架構與 [ASP.NET Core 2.1]。 選取 [API] 專案範本，然後按一下 [確定]：
1. 造訪 [NuGet 資源庫：MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) 來判斷 MongoDB 的最新穩定 .NET 驅動程式版本。 在 [套件管理員主控台] 視窗中，瀏覽到專案根目錄。 執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在命令殼層中執行下列命令：

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    會產生以 .NET Core 為目標的新 ASP.NET Core Web API 專案，並在 Visual Studio Code 中開啟。

1. 當 ['BooksApi' 中缺少要建置及偵錯的必要資產。是否要新增它們?] 通知出現時，請按一下 [是]。
1. 造訪 [NuGet 資源庫：MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) 來判斷 MongoDB 的最新穩定 .NET 驅動程式版本。 開啟 [整合式終端機] 並瀏覽到專案根目錄。 執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 移至 [檔案] > [新增解決方案] > [.NET Core] > [應用程式]。
1. 選取 **ASP.NET Core Web API** C# 專案範本，然後按一下 [下一步]。
1. 從 [目標 Framework] 下拉式清單選取 [.NET Core 2.2]，然後按一下 [下一步]。
1. 在 [專案名稱] 中輸入 *BooksApi*，然後按一下 [建立]。
1. 在 [方案] 台中，以滑鼠右鍵按一下專案的 [相依性] 節點並選取 [新增封裝]。
1. 在搜尋方塊中輸入 *MongoDB.Driver*、選取 *MongoDB.Driver* 套件，然後按一下 [新增封裝]。
1. 按一下 [授權接受] 對話方塊中的 [接受] 按鈕。

---

## <a name="add-a-model"></a>新增模型

1. 新增 *Models* 目錄到專案根目錄。
1. 新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

在上面的類別中，需要 `Id` 屬性：

* 才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。
* 使用 `[BsonId]` 加上附註以指定此屬性做為文件的主要機碼。
* 使用 `[BsonRepresentation(BsonType.ObjectId)]` 加上附註以允許將參數傳遞為類型 `string` 而非 `ObjectId`。 Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。

該類別中的其他屬性是使用 `[BsonElement]` 屬性加上註解。 屬性的值代表 MongoDB 集合中的屬性名稱。

## <a name="add-a-crud-operations-class"></a>新增 CRUD 作業類別

1. 新增 *Services* 目錄到專案根目錄。
1. 新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. 新增 MongoDB 連接字串到 *appsettings.json*：

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    上面的 `BookstoreDb` 屬性是在 `BookService` 類別建構函式中存取的。

1. 在 `Startup.ConfigureServices` 中，向相依性插入系統註冊 `BookService` 類別：

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    必須完成上面的服務註冊，才能在取用資源的類別中支援建構函式插入。

`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：

* `MongoClient` &ndash; 讀取用於執行資料庫作業的伺服器執行個體。 此類別的建構函式是使用 MongoDB 連接字串提供：

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; 代表用於執行作業的 Mongo 資料庫。 此教學課程使用介面上的一般 `GetCollection<T>(collection)` 方法來存取特定集合中的資料。 呼叫此方法之後，CRUD 作業可針對集合執行。 在 `GetCollection<T>(collection)` 方法呼叫中：
  * `collection` 代表集合名稱。
  * `T` 代表儲存在集合中的 CLR 物件類型。

`GetCollection<T>(collection)` 會傳回代表集合的 `MongoCollection` 物件。 在此教學課程中，會在集合上叫用下列方法：

* `Find<T>` &ndash; 傳回集合中符合所提供搜尋條件的所有文件。
* `InsertOne` &ndash; 插入提供的物件做為集合中的新文件。
* `ReplaceOne` &ndash; 使用提供的物件取代符合所提供搜尋條件的單一文件。
* `DeleteOne` &ndash; 刪除符合所提供搜尋條件的單一文件。

## <a name="add-a-controller"></a>新增控制器

1. 新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    上述 Web API 控制器會：

    * 使用 `BookService` 類別來執行 CRUD 作業。
    * 包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。
1. 建置和執行應用程式。
1. 在您的瀏覽器中瀏覽到 `http://localhost:<port>/api/books`。 會顯示下列 JSON 回應：

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

## <a name="next-steps"></a>後續步驟

如需有關建置 ASP.NET Core Web API 的詳細資訊，請參閱下列資源：

* <xref:web-api/index>
* <xref:web-api/action-return-types>
