# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>將檢視新增至 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中，您將修改 `HelloWorldController` 類別來使用 Razor 檢視範本檔案，完全封裝對用戶端產生 HTML 回應的處理序。

您可以使用 Razor 建立檢視範本檔案。 以 Razor 為基礎的檢視範本具有 *.cshtml* 副檔名。 它們提供了一種使用 C# 建立 HTML 輸出的簡潔方式。

`Index` 方法目前會傳回字串，內含在控制器類別中已直接書寫好的固定訊息。 在 `HelloWorldController` 類別中，以下列程式碼取代 `Index` 方法：

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

上述程式碼會傳回 `View` 物件。 它使用檢視範本來產生對瀏覽器的 HTML 回應。 如上述 `Index` 方法等控制器方法 (也稱為動作方法)，通常會傳回 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (或衍生自 `ActionResult` 的類別)，而不是如字串等類型。
