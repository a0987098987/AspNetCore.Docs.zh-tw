
## <a name="test-the-app"></a>測試應用程式

* 執行應用程式，然後點選 **Mvc Movie** 連結。
* 點選 **Create New** 連結，並建立電影。

  ![建立含有內容類型、價格、發行日期和標題等欄位的檢視](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* 您可能無法在 `Price` 欄位中輸入小數點或逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。 如需詳細資訊，請參閱 [https://github.com/aspnet/Docs/issues/4076](https://github.com/aspnet/Docs/issues/4076) 和[其他資源](#additional-resources)。 現在，只要輸入如 10 之類的整數。

<a name="displayformatdatelocal"></a>

* 在某些地區設定中，您需要指定日期格式。 請參閱下列強調顯示的程式碼。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

我們稍後將在本教學課程中討論 `DataAnnotations`。

點選 **Create** 會導致表單發佈至伺服器，該伺服器是電影資訊儲存在資料庫中的位置。 應用程式會重新導向至 */Movies* URL，其中顯示新建立的電影資訊。

![電影檢視顯示新建立的電影清單](~/tutorials/first-mvc-app/adding-model/_static/h.png)

建立幾個電影項目。 嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。
