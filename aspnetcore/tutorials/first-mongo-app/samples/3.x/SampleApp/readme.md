---
page_type: sample
description: 此教學課程示範如何使用 MongoDB NoSQL 資料庫建立 ASP.NET Core Web API。
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: aspnetcore-webapi-mongodb
ms.openlocfilehash: 09d73e25667822b8748a00cc76ad6d4f0e5fe290
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511401"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>使用 ASP.NET Core 與 MongoDB 建立 Web API

此教學課程會建立 Web API，此 API 會在 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 資料庫上執行建立、讀取、更新及刪除 (CRUD) 作業。

在本教學課程中，您會了解如何：

* 設定 MongoDB
* 建立 MongoDB 資料庫
* 定義 MongoDB 集合與結構描述
* 從 Web API 執行 MongoDB CRUD 作業
* 自訂 JSON 序列化

## <a name="prerequisites"></a>Prerequisites

* [.NET Core SDK 3.0 或更新版本](https://dotnet.microsoft.com/download/dotnet-core)
* [Visual Studio 2019 預覽版](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16&utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019preview)以及 **ASP.NET 與 Web 開發**工作負載
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

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

  > [!NOTE]
  > 您執行此範例時的識別碼，將不同於本文中顯示的識別碼。

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

1. 移至 [檔案] > [新增] > [專案]。
1. 選取 [ASP.NET Core Web 應用程式] 專案類型，然後選取 [下一步]。
1. 將專案命名為 *BooksApi*，然後選取 [建立]。
1. 選取 [.NET Core] 目標架構與 [ASP.NET Core 3.0]。 選取 [API] 專案範本，然後選取 [確定]。
1. 請造訪[NuGet 資源庫： MongoDB 驅動程式](https://www.nuget.org/packages/MongoDB.Driver/)，以判斷最新穩定版本的 .net Driver for MongoDB。 在 [套件管理員主控台] 視窗中，瀏覽到專案根目錄。 執行下列命令以安裝適用於 MongoDB 的 .NET 驅動程式：

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

## <a name="add-an-entity-model"></a>新增實體模型

1. 新增 *Models* 目錄到專案根目錄。
1. 新增具有下列程式碼的 `Book` 類別到 *Models* 目錄：

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

    在上面的類別中，需要 `Id` 屬性：

    * 才能將通用語言執行平台 (CLR) 物件對應到 MongoDB 集合。
    * 會以[`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm)標注，將此屬性指定為檔的主要金鑰。
    * 會以[`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm)標注，允許以類型 `string` （而不是[ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm)結構）傳遞參數。 Mongo 會處理從 `string` 轉換到 `ObjectId` 的作業。

    `BookName` 屬性是以[`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm)屬性來標注。 `Name` 的屬性值代表 MongoDB 集合中的屬性名稱。

## <a name="add-a-configuration-model"></a>新增組態模型

1. 將下列資料庫組態值新增至 *appsettings.json*：

    ```javascript
    {
      "BookstoreDatabaseSettings": {
        "BooksCollectionName": "Books",
        "ConnectionString": "mongodb://localhost:27017",
        "DatabaseName": "BookstoreDb"
      },

    ```

1. 使用下列程式碼將 *BookstoreDatabaseSettings.cs* 檔案新增至 *Models* 目錄：

    ```csharp
    namespace BooksApi.Models
    {
        public class BookstoreDatabaseSettings : IBookstoreDatabaseSettings
        {
            public string BooksCollectionName { get; set; }
            public string ConnectionString { get; set; }
            public string DatabaseName { get; set; }
        }

        public interface IBookstoreDatabaseSettings
        {
            string BooksCollectionName { get; set; }
            string ConnectionString { get; set; }
            string DatabaseName { get; set; }
        }
    }
    ```

    上述 `BookstoreDatabaseSettings` 類別用來儲存 *appsettings.json* 檔案的 `BookstoreDatabaseSettings` 屬性值。 JSON 和 C# 屬性名稱以相同方式命名，以簡化對應程序。

1. 將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddControllers();
    }
    ```

    在上述程式碼中：

    * *appsettings.json* 檔案之 `BookstoreDatabaseSettings` 區段所繫結的組態執行個體，是在相依性插入 (DI) 容器中註冊。 例如，`BookstoreDatabaseSettings` 物件的 `ConnectionString` 屬性會填入 `BookstoreDatabaseSettings:ConnectionString`appsettings.json*中的* 屬性。
    * `IBookstoreDatabaseSettings` 介面使用 singleton [服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)在 DI 中註冊。 插入時，介面執行個體會解析成 `BookstoreDatabaseSettings` 物件。

1. 在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookstoreDatabaseSettings` 和 `IBookstoreDatabaseSettings` 參考：

    ```csharp
    using BooksApi.Models;
    ```

## <a name="add-a-crud-operations-service"></a>新增 CRUD 作業服務

1. 新增 *Services* 目錄到專案根目錄。
1. 新增具有下列程式碼的 `BookService` 類別到 *Services* 目錄：

    ```csharp
    using BooksApi.Models;
    using MongoDB.Driver;
    using System.Collections.Generic;
    using System.Linq;

    namespace BooksApi.Services
    {
        public class BookService
        {
            private readonly IMongoCollection<Book> _books;

            public BookService(IBookstoreDatabaseSettings settings)
            {
                var client = new MongoClient(settings.ConnectionString);
                var database = client.GetDatabase(settings.DatabaseName);

                _books = database.GetCollection<Book>(settings.BooksCollectionName);
            }

            public List<Book> Get() =>
                _books.Find(book => true).ToList();

            public Book Get(string id) =>
                _books.Find<Book>(book => book.Id == id).FirstOrDefault();

            public Book Create(Book book)
            {
                _books.InsertOne(book);
                return book;
            }

            public void Update(string id, Book bookIn) =>
                _books.ReplaceOne(book => book.Id == id, bookIn);

            public void Remove(Book bookIn) =>
                _books.DeleteOne(book => book.Id == bookIn.Id);

            public void Remove(string id) =>
                _books.DeleteOne(book => book.Id == id);
        }
    }
    ```

    在上述程式碼中，透過建構函式插入從 DI 擷取 `IBookstoreDatabaseSettings` 執行個體。 這項技術可讓您存取*新增組態模型*一節中已新增的 [appsettings.json](#add-a-configuration-model) 組態值。

1. 將下列醒目提示的程式碼新增至 `Startup.ConfigureServices`：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddSingleton<BookService>();

        services.AddControllers();
    }
    ```

    在上述程式碼中，`BookService` 類別要向 DI 註冊才能在取用類別中支援建構函式插入。 因為 `BookService` 直接依存於 `MongoClient`，所以 Singleton 服務存留期最適合。 依照正式的 [Mongo 用戶端重複使用指導方針](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)，`MongoClient` 應該在 DI 註冊 Singleton 服務存留期。

1. 在 *Startup.cs* 的頂端新增下列程式碼，以解析 `BookService` 參考：


    ```csharp
    using BooksApi.Services;
    ```

`BookService` 類別使用下列 `MongoDB.Driver` 成員來對資料庫執行 CRUD 作業：

* [MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; 讀取伺服器實例以執行資料庫作業。 此類別的建構函式是使用 MongoDB 連接字串提供：

    ```csharp
    public BookService(IBookstoreDatabaseSettings settings)
    {
        var client = new MongoClient(settings.ConnectionString);
        var database = client.GetDatabase(settings.DatabaseName);

        _books = database.GetCollection<Book>(settings.BooksCollectionName);
    }
    ```

* [IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; 代表用於執行作業的 Mongo 資料庫。 本教學課程在介面上使用一般的 [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) 方法來存取特定集合中的資料。 呼叫此方法之後，針對集合執行 CRUD 作業。 在 `GetCollection<TDocument>(collection)` 方法呼叫中：
  * `collection` 代表集合名稱。
  * `TDocument` 代表儲存在集合中的 CLR 物件類型。

`GetCollection<TDocument>(collection)` 傳回代表集合的 [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) 物件。 在此教學課程中，會在集合上叫用下列方法：

* [Collection.deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; 會刪除符合所提供搜尋條件的單一檔。
* [尋找\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; 會傳回集合中符合所提供搜尋條件的所有檔。
* [InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; 會在集合中插入提供的物件做為新檔。
* [ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; 會以提供的物件取代符合所提供搜尋條件的單一檔。

## <a name="add-a-controller"></a>新增控制器

新增具有下列程式碼的 `BooksController` 類別到 *Controllers* 目錄：

```csharp
using BooksApi.Models;
using BooksApi.Services;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;

namespace BooksApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class BooksController : ControllerBase
    {
        private readonly BookService _bookService;

        public BooksController(BookService bookService)
        {
            _bookService = bookService;
        }

        [HttpGet]
        public ActionResult<List<Book>> Get() =>
            _bookService.Get();

        [HttpGet("{id:length(24)}", Name = "GetBook")]
        public ActionResult<Book> Get(string id)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            return book;
        }

        [HttpPost]
        public ActionResult<Book> Create(Book book)
        {
            _bookService.Create(book);

            return CreatedAtRoute("GetBook", new { id = book.Id.ToString() }, book);
        }

        [HttpPut("{id:length(24)}")]
        public IActionResult Update(string id, Book bookIn)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            _bookService.Update(id, bookIn);

            return NoContent();
        }

        [HttpDelete("{id:length(24)}")]
        public IActionResult Delete(string id)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            _bookService.Remove(book.Id);

            return NoContent();
        }
    }
}
```

上述 Web API 控制器會：

* 使用 `BookService` 類別來執行 CRUD 作業。
* 包含動作方法以支援 GET、POST、PUT 與 DELETE HTTP 要求。
* 在 <xref:System.Web.Http.ApiController.CreatedAtRoute*> 動作方法中呼叫 `Create`，以傳回 [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 回應。 對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是狀態碼 201。 `CreatedAtRoute` 也會將 `Location` 標頭新增至回應。 `Location` 標頭指定新建活頁簿的 URI。

## <a name="test-the-web-api"></a>測試 Web API

1. 建置並執行應用程式。

1. 巡覽至 `http://localhost:<port>/api/books`，測試控制器的無參數 `Get` 動作方法。 會顯示下列 JSON 回應：

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

1. 巡覽至 `http://localhost:<port>/api/books/{id here}`，測試控制器的多載 `Get` 動作方法。 會顯示下列 JSON 回應：

    ```json
    {
      "id":"{ID}",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a>設定 JSON 序列化選項

[測試 Web API](#test-the-web-api) 區段中傳回的 JSON 回應，有兩個詳細資訊要變更：

* 屬性名稱的預設駝峰式命名法大小寫應予變更，使其符合CLR 物件屬性名稱的 Pascal 命名法大小寫。
* `bookName` 屬性應傳回為 `Name`。

為滿足上述需求，請進行下列變更：

1. 已從 ASP.NET 共用架構移除 JSON.NET。 將套件參考新增至 [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson)。

1. 在 `Startup.ConfigureServices` 中，將下列醒目提示的程式碼鏈結至 `AddMvc` 方法呼叫：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddSingleton<BookService>();

        services.AddControllers()
            .AddNewtonsoftJson(options => options.UseMemberCasing());
    }
    ```

    完成前述變更後，Web API 序列化 JSON 回應中屬性名稱即符合其對應的 CLR 物件類型屬性名稱。 例如，`Book` 類別的 `Author` 屬性會序列化為 `Author`。

1. 在*模型/Book .cs*中，使用下列[`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm)屬性來標注 `BookName` 屬性：

    ```csharp
    [BsonElement("Name")]
    [JsonProperty("Name")]
    public string BookName { get; set; }
    ```

    `[JsonProperty]` 的屬性值 `Name` 代表 Web API 序列化 JSON 回應中的屬性名稱。

1. 在 *Models/Book.cs* 的頂端新增下列程式碼，以解析 `[JsonProperty]` 屬性參考：

    ```csharp
    using Newtonsoft.Json;
    ```

1. 重複[測試 Web API](#test-the-web-api) 一節中定義的步驟。 請注意 JSON 屬性名稱中的差異。

## <a name="next-steps"></a>後續步驟

如需有關建置 ASP.NET Core Web API 的詳細資訊，請參閱下列資源：

* [本文的 YouTube 版本](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* [使用 ASP.NET Core 建立 Web API](https://docs.microsoft.com/aspnet/core/web-api/index?view=aspnetcore-3.0)
