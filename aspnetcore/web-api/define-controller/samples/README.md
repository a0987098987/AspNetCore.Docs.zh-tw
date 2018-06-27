# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="99148-101">ASP.NET Core Web API 控制器範例</span><span class="sxs-lookup"><span data-stu-id="99148-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="99148-102">此範例應用程式包含下列專案：</span><span class="sxs-lookup"><span data-stu-id="99148-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="99148-103">**WebApiSample.Api**：以 .NET Core 2.1 為目標的 ASP.NET Core 2.1 專案。</span><span class="sxs-lookup"><span data-stu-id="99148-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="99148-104">**WebApiSample.Api.Pre21**：以 .NET Core 2.0 為目標的 ASP.NET Core 2.0 專案。</span><span class="sxs-lookup"><span data-stu-id="99148-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="99148-105">**WebApiSample.DataAccess**：用為 2 個 Web API 專案資料存取層的 .NET Standard 2.0 類別庫。</span><span class="sxs-lookup"><span data-stu-id="99148-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="99148-106">本範例說明建立 Web API 控制器的各種方式：</span><span class="sxs-lookup"><span data-stu-id="99148-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="99148-107">來自 ControllerBase 的衍生類別</span><span class="sxs-lookup"><span data-stu-id="99148-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [<span data-ttu-id="99148-108">以 ApiControllerAttribute 標註類別</span><span class="sxs-lookup"><span data-stu-id="99148-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)