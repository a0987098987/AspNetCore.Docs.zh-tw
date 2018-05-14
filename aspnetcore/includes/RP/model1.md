作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中，您可以新增類別來管理資料庫中的電影。 搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。 EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化您必須撰寫的資料存取程式碼。

您所建立的模型類別稱為 POCO 類別 (來自「純舊 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。 它們會定義資料儲存在資料庫中的屬性。

在本教學課程中，您首先要撰寫模型類別，而 EF Core 會建立資料庫。 本文未提及的替代方法是[從現有的資料庫產生模型類別](/ef/core/get-started/aspnetcore/existing-db)。

[檢視或下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)範例。