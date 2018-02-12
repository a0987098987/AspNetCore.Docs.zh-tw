---
title: "從 ASP.NET 移轉至 ASP.NET Core 2.0"
author: isaac2004
description: "可接受現有 ASP.NET MVC 或 Web API 應用程式移轉至 ASP.NET Core 2.0。"
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc2
ms.openlocfilehash: aa06200c6983f2c09a7271c8e8ce4b38f54163ad
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="6505c-103">從 ASP.NET 移轉至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="6505c-103">Migrating from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="6505c-104">作者：[Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="6505c-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="6505c-105">這篇文章可作為 ASP.NET 應用程式移轉至 ASP.NET Core 2.0 的參考指南。</span><span class="sxs-lookup"><span data-stu-id="6505c-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6505c-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="6505c-106">Prerequisites</span></span>

* <span data-ttu-id="6505c-107">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="6505c-107">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="6505c-108">目標 Framework</span><span class="sxs-lookup"><span data-stu-id="6505c-108">Target frameworks</span></span>
<span data-ttu-id="6505c-109">ASP.NET Core 2.0 專案讓開發人員能夠彈性以 .NET Core、.NET Framework 或兩者為目標。</span><span class="sxs-lookup"><span data-stu-id="6505c-109">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="6505c-110">請參閱[針對伺服器應用程式在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server)，判斷哪些目標架構最適合。</span><span class="sxs-lookup"><span data-stu-id="6505c-110">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="6505c-111">以 .NET Framework 為目標時，專案需要參考個別的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6505c-111">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="6505c-112">以 .NET Core 為目標，可讓您消除許多明確的套件參考，這點多虧了 ASP.NET Core 2.0 [中繼套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="6505c-112">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="6505c-113">在您的專案中安裝 `Microsoft.AspNetCore.All` 中繼套件：</span><span class="sxs-lookup"><span data-stu-id="6505c-113">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="6505c-114">使用中繼套件時，不使用應用程式部署中繼套件中參考的任何套件。</span><span class="sxs-lookup"><span data-stu-id="6505c-114">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="6505c-115">.NET Core 執行階段存放區包含這些資產，而且它們會先行編譯以改善效能。</span><span class="sxs-lookup"><span data-stu-id="6505c-115">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="6505c-116">如需詳細資料，請參閱 [ASP.NET Core 2.x 的 Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="6505c-116">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="6505c-117">專案結構差異</span><span class="sxs-lookup"><span data-stu-id="6505c-117">Project structure differences</span></span>
<span data-ttu-id="6505c-118">ASP.NET Core 中已簡化 *.csproj* 檔案格式。</span><span class="sxs-lookup"><span data-stu-id="6505c-118">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="6505c-119">值得注意的變更包括：</span><span class="sxs-lookup"><span data-stu-id="6505c-119">Some notable changes include:</span></span>
- <span data-ttu-id="6505c-120">檔案不需要明確包含也會被視為專案的一部分。</span><span class="sxs-lookup"><span data-stu-id="6505c-120">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="6505c-121">在處理大型小組時，這會減少 XML 合併衝突的風險。</span><span class="sxs-lookup"><span data-stu-id="6505c-121">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="6505c-122">其他專案沒有可改善檔案可讀性之以 GUID 為基礎的參考。</span><span class="sxs-lookup"><span data-stu-id="6505c-122">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="6505c-123">您可以編輯檔案，卻不用在 Visual Studio 中卸載它：</span><span class="sxs-lookup"><span data-stu-id="6505c-123">The file can be edited without unloading it in Visual Studio:</span></span>

    ![在 Visual Studio 2017 中編輯 CSPROJ 操作功能表選項](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="6505c-125">取代 Global.asax 檔案</span><span class="sxs-lookup"><span data-stu-id="6505c-125">Global.asax file replacement</span></span>
<span data-ttu-id="6505c-126">ASP.NET Core 導入了啟動應用程式的新機制。</span><span class="sxs-lookup"><span data-stu-id="6505c-126">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="6505c-127">ASP.NET 應用程式的進入點是 *Global.asax* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6505c-127">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="6505c-128">路由組態和篩選器和區域登錄等工作，會在 *Global.asax* 檔案中處理。</span><span class="sxs-lookup"><span data-stu-id="6505c-128">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[Main](samples/globalasax-sample.cs)]

<span data-ttu-id="6505c-129">這個方法是以會影響到實作的方式，將應用程式和其部署所在的伺服器結合在一起。</span><span class="sxs-lookup"><span data-stu-id="6505c-129">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="6505c-130">為將它們分開，引進了 [OWIN](http://owin.org/) 以提供簡潔的方式，同時使用多個架構。</span><span class="sxs-lookup"><span data-stu-id="6505c-130">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="6505c-131">OWIN 提供的管線只新增所需的模組。</span><span class="sxs-lookup"><span data-stu-id="6505c-131">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="6505c-132">裝載環境採用 [Startup](xref:fundamentals/startup) 函式，設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="6505c-132">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="6505c-133">`Startup` 向應用程式註冊一組中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6505c-133">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="6505c-134">對於每項要求，應用程式會使用現有處理常式集合連結清單的標頭指標，呼叫每個中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="6505c-134">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="6505c-135">每個中介軟體元件都可以在要求處理管線新增一或多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6505c-135">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="6505c-136">這項作業是透過將參考傳回處理常式所完成，而此處理常式為清單的新標頭。</span><span class="sxs-lookup"><span data-stu-id="6505c-136">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="6505c-137">每個處理常式都負責記住和叫用清單中的下一個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6505c-137">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="6505c-138">使用 ASP.NET Core，應用程式的進入點是 `Startup`，對 *Global.asax* 不會再有相依性。</span><span class="sxs-lookup"><span data-stu-id="6505c-138">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="6505c-139">使用 OWIN 和 .NET Framework 時，請將類似下列的項目當成管線使用：</span><span class="sxs-lookup"><span data-stu-id="6505c-139">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[Main](samples/webapi-owin.cs)]

<span data-ttu-id="6505c-140">這會設定您的預設路由，並透過 JSON 將 XmlSerialization 設為預設值。</span><span class="sxs-lookup"><span data-stu-id="6505c-140">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="6505c-141">視需要在此管線新增其他中介軟體 (載入服務、組態設定、靜態檔案等等)。</span><span class="sxs-lookup"><span data-stu-id="6505c-141">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="6505c-142">ASP.NET Core 使用類似的方法，但不依賴 OWIN 處理項目。</span><span class="sxs-lookup"><span data-stu-id="6505c-142">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="6505c-143">相反地，這會透過 *Program.cs* `Main` 方法完成 (類似主控台應用程式)，而 `Startup` 也是透過該處載入。</span><span class="sxs-lookup"><span data-stu-id="6505c-143">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[Main](samples/program.cs)]

<span data-ttu-id="6505c-144">`Startup` 必須包含 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="6505c-144">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="6505c-145">在 `Configure` 中將必要的中介軟體新增至管線。</span><span class="sxs-lookup"><span data-stu-id="6505c-145">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="6505c-146">在下列範例中 (從預設的網站範本)，會使用數個擴充方法設定管線，以支援：</span><span class="sxs-lookup"><span data-stu-id="6505c-146">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="6505c-147">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="6505c-147">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="6505c-148">錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="6505c-148">Error pages</span></span>
* <span data-ttu-id="6505c-149">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="6505c-149">Static files</span></span>
* <span data-ttu-id="6505c-150">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6505c-150">ASP.NET Core MVC</span></span>
* <span data-ttu-id="6505c-151">身分識別</span><span class="sxs-lookup"><span data-stu-id="6505c-151">Identity</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="6505c-152">主機與應用程式已分離，這讓您未來可以彈性移至不同的平台。</span><span class="sxs-lookup"><span data-stu-id="6505c-152">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="6505c-153">**注意：**如需 ASP.NET Core 啟動和中介軟體的深入參考，請參閱 [ASP.NET Core 中的啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="6505c-153">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="6505c-154">正在儲存組態</span><span class="sxs-lookup"><span data-stu-id="6505c-154">Storing configurations</span></span>
<span data-ttu-id="6505c-155">ASP.NET 支援儲存設定。</span><span class="sxs-lookup"><span data-stu-id="6505c-155">ASP.NET supports storing settings.</span></span> <span data-ttu-id="6505c-156">例如，這些設定是用來支援要部署應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="6505c-156">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="6505c-157">過去的常見做法是將所有自訂機碼值組儲存在 *Web.config* 檔案的 `<appSettings>` 區段中：</span><span class="sxs-lookup"><span data-stu-id="6505c-157">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[Main](samples/webconfig-sample.xml)]

<span data-ttu-id="6505c-158">應用程式會讀取 `System.Configuration` 命名空間中這些使用 `ConfigurationManager.AppSettings` 集合的設定：</span><span class="sxs-lookup"><span data-stu-id="6505c-158">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[Main](samples/read-webconfig.cs)]

<span data-ttu-id="6505c-159">ASP.NET Core 可將應用程式的組態資料儲存在任何檔案中，將它們當成中介軟體啟動程序的一部分載入。</span><span class="sxs-lookup"><span data-stu-id="6505c-159">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="6505c-160">專案範本中所用的預設檔案是 *appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="6505c-160">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[Main](samples/appsettings-sample.json)]

<span data-ttu-id="6505c-161">將此檔案載入應用程式內的 `IConfiguration` 執行個體，是在 *Startup.cs* 中完成：</span><span class="sxs-lookup"><span data-stu-id="6505c-161">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[Main](samples/startup-builder.cs)]

<span data-ttu-id="6505c-162">應用程式讀取自 `Configuration` 以取得設定：</span><span class="sxs-lookup"><span data-stu-id="6505c-162">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[Main](samples/read-appsettings.cs)]

<span data-ttu-id="6505c-163">此方法有延伸模組可讓處理序更強固，例如使用[相依性插入](xref:fundamentals/dependency-injection) (DI) 載入具有這些值的服務。</span><span class="sxs-lookup"><span data-stu-id="6505c-163">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="6505c-164">DI 方法提供強型別的組態物件集合。</span><span class="sxs-lookup"><span data-stu-id="6505c-164">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="6505c-165">**注意：**如需 ASP.NET Core 組態的更深入參考，請參閱 [ASP.NET Core 的組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="6505c-165">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="6505c-166">原生相依性插入</span><span class="sxs-lookup"><span data-stu-id="6505c-166">Native dependency injection</span></span>
<span data-ttu-id="6505c-167">建置可延展的大型應用程式時，鬆散的元件和服務結合程度就是重要的目標。</span><span class="sxs-lookup"><span data-stu-id="6505c-167">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="6505c-168">[相依性插入](xref:fundamentals/dependency-injection)是達到此目標的常用技巧，它也是 ASP.NET Core 的原生元件。</span><span class="sxs-lookup"><span data-stu-id="6505c-168">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="6505c-169">在 ASP.NET 應用程式中，開發人員會依賴協力廠商程式庫實作相依性插入。</span><span class="sxs-lookup"><span data-stu-id="6505c-169">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="6505c-170">Microsoft 模式和實務提供的 [Unity](https://github.com/unitycontainer/unity) 就是這樣的程式庫。</span><span class="sxs-lookup"><span data-stu-id="6505c-170">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="6505c-171">使用 Unity 設定相依性插入的範例，是實作包裝 `UnityContainer` 的 `IDependencyResolver`：</span><span class="sxs-lookup"><span data-stu-id="6505c-171">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="6505c-172">建立您 `UnityContainer` 的執行個體、註冊您的服務，以及為容器設定 `UnityResolver` 新執行個體的 `HttpConfiguration` 相依性解析程式：</span><span class="sxs-lookup"><span data-stu-id="6505c-172">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="6505c-173">在需要的位置插入 `IProductRepository`：</span><span class="sxs-lookup"><span data-stu-id="6505c-173">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="6505c-174">因為相依性插入是 ASP.NET Core 的一部分，所以您可以在 *Startup.cs* 的 `ConfigureServices` 方法中新增服務：</span><span class="sxs-lookup"><span data-stu-id="6505c-174">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](samples/configure-services.cs)]

<span data-ttu-id="6505c-175">存放庫可插入任何位置，就像以前的 Unity 一樣。</span><span class="sxs-lookup"><span data-stu-id="6505c-175">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="6505c-176">**注意：**如需 ASP.NET Core 中相依性插入的深入參考，請參閱 [ASP.NET Core 中的相依性插入](xref:fundamentals/dependency-injection#replacing-the-default-services-container)。</span><span class="sxs-lookup"><span data-stu-id="6505c-176">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="6505c-177">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="6505c-177">Serving static files</span></span>
<span data-ttu-id="6505c-178">網頁程式開發很重要的一部分是能夠提供靜態的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="6505c-178">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="6505c-179">最常見的靜態檔案範例包括 HTML、CSS、Javascript 和影像。</span><span class="sxs-lookup"><span data-stu-id="6505c-179">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="6505c-180">這些檔案需要儲存在應用程式 (或 CDN) 的發佈位置供參考，以便要求可以載入它們。</span><span class="sxs-lookup"><span data-stu-id="6505c-180">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="6505c-181">此程序在 ASP.NET Core 中已變更。</span><span class="sxs-lookup"><span data-stu-id="6505c-181">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="6505c-182">在 ASP.NET 中，靜態檔案會儲存在不同目錄中，於檢視中提供參考。</span><span class="sxs-lookup"><span data-stu-id="6505c-182">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="6505c-183">在 ASP.NET Core 中，靜態檔案會儲存在「Web 根目錄」(*&lt;內容根目錄&gt;/wwwroot*) 中，除非另行設定。</span><span class="sxs-lookup"><span data-stu-id="6505c-183">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="6505c-184">從 `Startup.Configure` 叫用 `UseStaticFiles` 擴充方法，將檔案載入至要求管線：</span><span class="sxs-lookup"><span data-stu-id="6505c-184">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="6505c-185">**注意：**如以 .NET Framework 為目標，請安裝 NuGet 套件 `Microsoft.AspNetCore.StaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="6505c-185">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="6505c-186">例如，位在 `http://<app>/images/<imageFileName>` 等位置的瀏覽器可存取 *wwwroot/images* 資料夾中的影像資產。</span><span class="sxs-lookup"><span data-stu-id="6505c-186">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="6505c-187">**注意：**如需在 ASP.NET Core 中提供靜態檔案的更深入參考，請參閱[在 ASP.NET Core 中使用靜態檔案的簡介](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="6505c-187">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6505c-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="6505c-188">Additional resources</span></span>

* [<span data-ttu-id="6505c-189">將程式庫移轉到 .NET Core</span><span class="sxs-lookup"><span data-stu-id="6505c-189">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
