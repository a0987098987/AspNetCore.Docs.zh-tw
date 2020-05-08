---
title: 從 ASP.NET 遷移至 ASP.NET Core 2.0
author: isaac2004
description: 接收將現有的 ASP.NET MVC 或 Web API 應用程式遷移至 ASP.NET Core 2.0 的指引。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/mvc2
ms.openlocfilehash: 98b93731d40b47be8d7d5050afc7c74d25a7e1dc
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776268"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="105ff-103">從 ASP.NET 遷移至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="105ff-103">Migrate from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="105ff-104">作者：[Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="105ff-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="105ff-105">這篇文章可作為 ASP.NET 應用程式移轉至 ASP.NET Core 2.0 的參考指南。</span><span class="sxs-lookup"><span data-stu-id="105ff-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="105ff-106">先決條件</span><span class="sxs-lookup"><span data-stu-id="105ff-106">Prerequisites</span></span>

<span data-ttu-id="105ff-107">從 .Net 下載安裝下列**其中一**項[： Windows](https://dotnet.microsoft.com/download)：</span><span class="sxs-lookup"><span data-stu-id="105ff-107">Install **one** of the following from [.NET Downloads: Windows](https://dotnet.microsoft.com/download):</span></span>

* <span data-ttu-id="105ff-108">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="105ff-108">.NET Core SDK</span></span>
* <span data-ttu-id="105ff-109">適用於 Windows 的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="105ff-109">Visual Studio for Windows</span></span>
  * <span data-ttu-id="105ff-110">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="105ff-110">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="105ff-111">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="105ff-111">**.NET Core cross-platform development** workload</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="105ff-112">目標 Framework</span><span class="sxs-lookup"><span data-stu-id="105ff-112">Target frameworks</span></span>

<span data-ttu-id="105ff-113">ASP.NET Core 2.0 專案讓開發人員能夠彈性以 .NET Core、.NET Framework 或兩者為目標。</span><span class="sxs-lookup"><span data-stu-id="105ff-113">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="105ff-114">請參閱[針對伺服器應用程式在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/standard/choosing-core-framework-server)，判斷哪些目標架構最適合。</span><span class="sxs-lookup"><span data-stu-id="105ff-114">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="105ff-115">以 .NET Framework 為目標時，專案需要參考個別的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="105ff-115">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="105ff-116">以 .NET Core 為目標，可讓您消除許多明確的套件參考，這點多虧了 ASP.NET Core 2.0 [中繼套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="105ff-116">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="105ff-117">在您的專案中安裝 `Microsoft.AspNetCore.All` 中繼套件：</span><span class="sxs-lookup"><span data-stu-id="105ff-117">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
</ItemGroup>
```

<span data-ttu-id="105ff-118">使用中繼套件時，不使用應用程式部署中繼套件中參考的任何套件。</span><span class="sxs-lookup"><span data-stu-id="105ff-118">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="105ff-119">.NET Core 執行階段存放區包含這些資產，而且它們會先行編譯以改善效能。</span><span class="sxs-lookup"><span data-stu-id="105ff-119">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="105ff-120">如<xref:fundamentals/metapackage>需詳細資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="105ff-120">See <xref:fundamentals/metapackage> for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="105ff-121">專案結構差異</span><span class="sxs-lookup"><span data-stu-id="105ff-121">Project structure differences</span></span>

<span data-ttu-id="105ff-122">ASP.NET Core 中已簡化 *.csproj* 檔案格式。</span><span class="sxs-lookup"><span data-stu-id="105ff-122">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="105ff-123">值得注意的變更包括：</span><span class="sxs-lookup"><span data-stu-id="105ff-123">Some notable changes include:</span></span>

* <span data-ttu-id="105ff-124">檔案不需要明確包含也會被視為專案的一部分。</span><span class="sxs-lookup"><span data-stu-id="105ff-124">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="105ff-125">在處理大型小組時，這會減少 XML 合併衝突的風險。</span><span class="sxs-lookup"><span data-stu-id="105ff-125">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
* <span data-ttu-id="105ff-126">其他專案沒有可改善檔案可讀性之以 GUID 為基礎的參考。</span><span class="sxs-lookup"><span data-stu-id="105ff-126">There are no GUID-based references to other projects, which improves file readability.</span></span>
* <span data-ttu-id="105ff-127">您可以編輯檔案，卻不用在 Visual Studio 中卸載它：</span><span class="sxs-lookup"><span data-stu-id="105ff-127">The file can be edited without unloading it in Visual Studio:</span></span>

  ![在 Visual Studio 2017 中編輯 CSPROJ 操作功能表選項](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="105ff-129">取代 Global.asax 檔案</span><span class="sxs-lookup"><span data-stu-id="105ff-129">Global.asax file replacement</span></span>

<span data-ttu-id="105ff-130">ASP.NET Core 導入了啟動應用程式的新機制。</span><span class="sxs-lookup"><span data-stu-id="105ff-130">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="105ff-131">ASP.NET 應用程式的進入點是 *Global.asax* 檔案。</span><span class="sxs-lookup"><span data-stu-id="105ff-131">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="105ff-132">路由組態和篩選器和區域登錄等工作，會在 *Global.asax* 檔案中處理。</span><span class="sxs-lookup"><span data-stu-id="105ff-132">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="105ff-133">這個方法是以會影響到實作的方式，將應用程式和其部署所在的伺服器結合在一起。</span><span class="sxs-lookup"><span data-stu-id="105ff-133">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="105ff-134">為將它們分開，引進了 [OWIN](https://owin.org/) 以提供簡潔的方式，同時使用多個架構。</span><span class="sxs-lookup"><span data-stu-id="105ff-134">In an effort to decouple, [OWIN](https://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="105ff-135">OWIN 提供的管線只新增所需的模組。</span><span class="sxs-lookup"><span data-stu-id="105ff-135">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="105ff-136">裝載環境採用 [Startup](xref:fundamentals/startup) 函式，設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="105ff-136">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="105ff-137">`Startup` 向應用程式註冊一組中介軟體。</span><span class="sxs-lookup"><span data-stu-id="105ff-137">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="105ff-138">對於每項要求，應用程式會使用現有處理常式集合連結清單的標頭指標，呼叫每個中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="105ff-138">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="105ff-139">每個中介軟體元件都可以在要求處理管線新增一或多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="105ff-139">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="105ff-140">這項作業是透過將參考傳回處理常式所完成，而此處理常式為清單的新標頭。</span><span class="sxs-lookup"><span data-stu-id="105ff-140">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="105ff-141">每個處理常式都負責記住和叫用清單中的下一個處理常式。</span><span class="sxs-lookup"><span data-stu-id="105ff-141">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="105ff-142">使用 ASP.NET Core，應用程式的進入點是 `Startup`，對 *Global.asax* 不會再有相依性。</span><span class="sxs-lookup"><span data-stu-id="105ff-142">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="105ff-143">使用 OWIN 和 .NET Framework 時，請將類似下列的項目當成管線使用：</span><span class="sxs-lookup"><span data-stu-id="105ff-143">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="105ff-144">這會設定您的預設路由，並透過 JSON 將 XmlSerialization 設為預設值。</span><span class="sxs-lookup"><span data-stu-id="105ff-144">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="105ff-145">視需要在此管線新增其他中介軟體 (載入服務、組態設定、靜態檔案等等)。</span><span class="sxs-lookup"><span data-stu-id="105ff-145">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="105ff-146">ASP.NET Core 使用類似的方法，但不依賴 OWIN 處理項目。</span><span class="sxs-lookup"><span data-stu-id="105ff-146">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="105ff-147">相反地，這會透過 *Program.cs* `Main` 方法完成 (類似主控台應用程式)，而 `Startup` 也是透過該處載入。</span><span class="sxs-lookup"><span data-stu-id="105ff-147">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="105ff-148">`Startup` 必須包含 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="105ff-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="105ff-149">在 `Configure` 中將必要的中介軟體新增至管線。</span><span class="sxs-lookup"><span data-stu-id="105ff-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="105ff-150">在下列範例中 (從預設的網站範本)，會使用數個擴充方法設定管線，以支援：</span><span class="sxs-lookup"><span data-stu-id="105ff-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="105ff-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="105ff-151">BrowserLink</span></span>](https://vswebessentials.com/features/browserlink)
* <span data-ttu-id="105ff-152">錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="105ff-152">Error pages</span></span>
* <span data-ttu-id="105ff-153">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="105ff-153">Static files</span></span>
* <span data-ttu-id="105ff-154">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="105ff-154">ASP.NET Core MVC</span></span>
* Identity

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="105ff-155">主機與應用程式已分離，這讓您未來可以彈性移至不同的平台。</span><span class="sxs-lookup"><span data-stu-id="105ff-155">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="105ff-156">如需 ASP.NET Core 啟動和中介軟體的更深入參考，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="105ff-156">For a more in-depth reference to ASP.NET Core Startup and Middleware, see <xref:fundamentals/startup>.</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="105ff-157">正在儲存組態</span><span class="sxs-lookup"><span data-stu-id="105ff-157">Storing configurations</span></span>

<span data-ttu-id="105ff-158">ASP.NET 支援儲存設定。</span><span class="sxs-lookup"><span data-stu-id="105ff-158">ASP.NET supports storing settings.</span></span> <span data-ttu-id="105ff-159">例如，這些設定是用來支援要部署應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="105ff-159">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="105ff-160">過去的常見做法是將所有自訂機碼值組儲存在 *Web.config* 檔案的 `<appSettings>` 區段中：</span><span class="sxs-lookup"><span data-stu-id="105ff-160">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="105ff-161">應用程式會讀取 `System.Configuration` 命名空間中這些使用 `ConfigurationManager.AppSettings` 集合的設定：</span><span class="sxs-lookup"><span data-stu-id="105ff-161">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="105ff-162">ASP.NET Core 可將應用程式的組態資料儲存在任何檔案中，將它們當成中介軟體啟動程序的一部分載入。</span><span class="sxs-lookup"><span data-stu-id="105ff-162">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="105ff-163">專案範本中所用的預設檔案是 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="105ff-163">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="105ff-164">將此檔案載入應用程式內的 `IConfiguration` 執行個體，是在 *Startup.cs* 中完成：</span><span class="sxs-lookup"><span data-stu-id="105ff-164">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="105ff-165">應用程式讀取自 `Configuration` 以取得設定：</span><span class="sxs-lookup"><span data-stu-id="105ff-165">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="105ff-166">此方法有延伸模組可讓處理序更強固，例如使用[相依性插入](xref:fundamentals/dependency-injection) (DI) 載入具有這些值的服務。</span><span class="sxs-lookup"><span data-stu-id="105ff-166">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="105ff-167">DI 方法提供強型別的組態物件集合。</span><span class="sxs-lookup"><span data-stu-id="105ff-167">The DI approach provides a strongly-typed set of configuration objects.</span></span>

```csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
```

<span data-ttu-id="105ff-168">**注意：** 如需 ASP.NET Core 設定的更深入參考，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="105ff-168">**Note:** For a more in-depth reference to ASP.NET Core configuration, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="105ff-169">原生相依性插入</span><span class="sxs-lookup"><span data-stu-id="105ff-169">Native dependency injection</span></span>

<span data-ttu-id="105ff-170">建置可延展的大型應用程式時，鬆散的元件和服務結合程度就是重要的目標。</span><span class="sxs-lookup"><span data-stu-id="105ff-170">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="105ff-171">相依性[插入](xref:fundamentals/dependency-injection)是達到此目標的常用技巧，而它是 ASP.NET Core 的原生元件。</span><span class="sxs-lookup"><span data-stu-id="105ff-171">[Dependency injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="105ff-172">在 ASP.NET 應用程式中，開發人員依賴協力廠商程式庫來執行相依性插入。</span><span class="sxs-lookup"><span data-stu-id="105ff-172">In ASP.NET applications, developers rely on a third-party library to implement dependency injection.</span></span> <span data-ttu-id="105ff-173">Microsoft 模式和實務提供的 [Unity](https://github.com/unitycontainer/unity) 就是這樣的程式庫。</span><span class="sxs-lookup"><span data-stu-id="105ff-173">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="105ff-174">使用 Unity 設定相依性插入的範例，就是`IDependencyResolver`執行包裝 a `UnityContainer`的：</span><span class="sxs-lookup"><span data-stu-id="105ff-174">An example of setting up dependency injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](samples/sample8.cs)]

<span data-ttu-id="105ff-175">建立您 `UnityContainer` 的執行個體、註冊您的服務，以及為容器設定 `UnityResolver` 新執行個體的 `HttpConfiguration` 相依性解析程式：</span><span class="sxs-lookup"><span data-stu-id="105ff-175">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](samples/sample9.cs)]

<span data-ttu-id="105ff-176">在需要的位置插入 `IProductRepository`：</span><span class="sxs-lookup"><span data-stu-id="105ff-176">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](samples/sample5.cs)]

<span data-ttu-id="105ff-177">由於相依性插入是 ASP.NET Core 的一部分，因此您可以在中新增`Startup.ConfigureServices`服務：</span><span class="sxs-lookup"><span data-stu-id="105ff-177">Because dependency injection is part of ASP.NET Core, you can add your service in the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="105ff-178">存放庫可插入任何位置，就像以前的 Unity 一樣。</span><span class="sxs-lookup"><span data-stu-id="105ff-178">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="105ff-179">如需 ASP.NET Core 中相依性插入的詳細資訊<xref:fundamentals/dependency-injection>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="105ff-179">For more information on dependency injection in ASP.NET Core, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="105ff-180">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="105ff-180">Serving static files</span></span>

<span data-ttu-id="105ff-181">網頁程式開發很重要的一部分是能夠提供靜態的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="105ff-181">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="105ff-182">最常見的靜態檔案範例包括 HTML、CSS、Javascript 和影像。</span><span class="sxs-lookup"><span data-stu-id="105ff-182">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="105ff-183">這些檔案需要儲存在應用程式 (或 CDN) 的發佈位置供參考，以便要求可以載入它們。</span><span class="sxs-lookup"><span data-stu-id="105ff-183">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="105ff-184">此程序在 ASP.NET Core 中已變更。</span><span class="sxs-lookup"><span data-stu-id="105ff-184">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="105ff-185">在 ASP.NET 中，靜態檔案會儲存在不同目錄中，於檢視中提供參考。</span><span class="sxs-lookup"><span data-stu-id="105ff-185">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="105ff-186">在 ASP.NET Core 中，除非另有設定，否則靜態檔案會儲存在「web 根目錄」（*&lt;內容根目錄&gt;/wwwroot*）中。</span><span class="sxs-lookup"><span data-stu-id="105ff-186">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="105ff-187">從 `Startup.Configure` 叫用 `UseStaticFiles` 擴充方法，將檔案載入至要求管線：</span><span class="sxs-lookup"><span data-stu-id="105ff-187">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="105ff-188">**注意：** 如以 .NET Framework 為目標，請安裝 NuGet 套件 `Microsoft.AspNetCore.StaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="105ff-188">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="105ff-189">例如，位在 `http://<app>/images/<imageFileName>` 等位置的瀏覽器可存取 *wwwroot/images* 資料夾中的影像資產。</span><span class="sxs-lookup"><span data-stu-id="105ff-189">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="105ff-190">**注意：** 如需在 ASP.NET Core 中提供靜態檔案的更深入參考，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="105ff-190">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see <xref:fundamentals/static-files>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="105ff-191">其他資源</span><span class="sxs-lookup"><span data-stu-id="105ff-191">Additional resources</span></span>

* [<span data-ttu-id="105ff-192">將程式庫移轉到 .NET Core</span><span class="sxs-lookup"><span data-stu-id="105ff-192">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
