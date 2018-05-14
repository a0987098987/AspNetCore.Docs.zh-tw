# <a name="adding-a-model-to-an-aspnet-core-mvc-app"></a>將模型加新增至 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/tdykstra)

在本節中，您將新增一些類別來管理資料庫中的電影。 這些類別是 **MVC** 應用程式的「模型」部分。

搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。 EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化您必須撰寫的資料存取程式碼。 [EF Core 支援許多資料庫引擎](/ef/core/providers/)。

您所建立的模型類別稱為 POCO 類別 (來自「純舊 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。 它們只會定義資料庫將儲存之資料的屬性。

在本教學課程中，您首先要撰寫模型類別，而 EF Core 將建立資料庫。 本文未提及的替代方法是從現有的資料庫產生模型類別。 如需該方法的資訊，請參閱 [ASP.NET Core - 現有的資料庫](/ef/core/get-started/aspnetcore/existing-db)。

## <a name="add-a-data-model-class"></a>新增資料模型類別
