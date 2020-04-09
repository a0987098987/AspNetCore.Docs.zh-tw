<!-- THIS INCLUDE USED BY MVC AND RP -->
將下列屬性新增至 `Movie` 類別：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

`Movie` 類別包含：

* `ID` 欄位是資料庫對於主索引鍵的必要欄位。
* `[DataType(DataType.Date)]`[:DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter)屬性指定數據類型(日期)。 使用此屬性：

  * 使用者不需要在日期欄位中輸入時間資訊。
  * 只會顯示日期，不會顯示時間資訊。

稍後的教學課程會涵蓋 [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)。
