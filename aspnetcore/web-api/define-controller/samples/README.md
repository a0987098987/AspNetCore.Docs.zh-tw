# <a name="aspnet-core-web-api-controller-sample"></a>ASP.NET Core Web API 控制器範例

此範例應用程式包含下列專案：

- **WebApiSample.Api**：以 .NET Core 2.1 為目標的 ASP.NET Core 2.1 專案。
- **WebApiSample.Api.Pre21**：以 .NET Core 2.0 為目標的 ASP.NET Core 2.0 專案。
- **WebApiSample.DataAccess**：用為 2 個 Web API 專案資料存取層的 .NET Standard 2.0 類別庫。

本範例說明建立 Web API 控制器的各種方式：

- [來自 ControllerBase 的衍生類別](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [以 ApiControllerAttribute 標註類別](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
