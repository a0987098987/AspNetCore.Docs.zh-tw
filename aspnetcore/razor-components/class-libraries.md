---
title: Razor 元件類別庫
author: guardrex
description: 探索如何包含元件，外部元件程式庫中的 Razor 元件應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515391"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="3f9eb-103">Razor 元件類別庫</span><span class="sxs-lookup"><span data-stu-id="3f9eb-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="3f9eb-104">藉由[Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="3f9eb-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="3f9eb-105">元件可以在專案之間共用 Razor 類別庫中。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="3f9eb-106">元件可以包含：</span><span class="sxs-lookup"><span data-stu-id="3f9eb-106">Components can be included from:</span></span>

* <span data-ttu-id="3f9eb-107">在方案中的另一個專案。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-107">Another project in the solution.</span></span>
* <span data-ttu-id="3f9eb-108">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-108">A NuGet package.</span></span>
* <span data-ttu-id="3f9eb-109">參考的.NET 程式庫。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-109">A referenced .NET library.</span></span>

<span data-ttu-id="3f9eb-110">元件是一般的.NET 型別，如同 Razor 類別庫所提供的元件就會是一般的.NET 組件。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="3f9eb-111">使用`razorclasslib`（Razor 類別程式庫） 範本，內含[dotnet 新](/dotnet/core/tools/dotnet-new)命令：</span><span class="sxs-lookup"><span data-stu-id="3f9eb-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="3f9eb-112">新增 Razor 元件檔案 (*.razor*) Razor 類別庫。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="3f9eb-113">若要加入現有的專案程式庫，請使用[dotnet sln](/dotnet/core/tools/dotnet-sln)命令：</span><span class="sxs-lookup"><span data-stu-id="3f9eb-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f9eb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f9eb-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f9eb-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3f9eb-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="3f9eb-116">Razor 類別庫與不相容的 ASP.NET Core 的 Preview 3 的 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="3f9eb-117">若要建立可以與 Blazor 和 Razor 元件的應用程式共用程式庫中的元件，請使用 Blazor 類別庫所建立`blazorlib`範本。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-117">To create components in a library that can be shared with Blazor and Razor Components apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="3f9eb-118">Razor 類別庫不支援 ASP.NET Core 的 Preview 3 中的靜態資產。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="3f9eb-119">元件程式庫使用`blazorlib`範本可以包含靜態檔案，例如影像、 JavaScript 和樣式表。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="3f9eb-120">在建置時，靜態檔案內嵌到建置組件檔案 (*.dll*)，可讓元件的耗用量，而不必擔心如何包含其資源。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="3f9eb-121">包含的任何檔案`content`目錄標示為內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="3f9eb-122">使用程式庫元件</span><span class="sxs-lookup"><span data-stu-id="3f9eb-122">Consume a library component</span></span>

<span data-ttu-id="3f9eb-123">若要使用定義在另一個專案中，文件庫中的元件[ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label)必須使用指示詞。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="3f9eb-124">個別元件可能會依名稱加入。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-124">Individual components may be added by name.</span></span>

<span data-ttu-id="3f9eb-125">指示詞一般格式為：</span><span class="sxs-lookup"><span data-stu-id="3f9eb-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="3f9eb-126">例如，新增下列指示詞`Component1`的`MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="3f9eb-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="3f9eb-127">不過，通常會包含所有的元件，從組件使用萬用字元 (`*`):</span><span class="sxs-lookup"><span data-stu-id="3f9eb-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="3f9eb-128">`@addTagHelper`指示詞可以包含在 *_ViewImport.cshtml*使元件適用於整個專案或套用至單一頁面或一組資料夾內的頁面。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="3f9eb-129">使用`@addTagHelper`就地指示詞時，元件程式庫的元件可以使用，彷彿應用程式相同的組件中。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="3f9eb-130">組建、 套件及出貨至 NuGet</span><span class="sxs-lookup"><span data-stu-id="3f9eb-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="3f9eb-131">因為元件程式庫是標準的.NET 程式庫，並無不同封裝和傳送任何程式庫 nuget 封裝和傳送至 NuGet。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="3f9eb-132">使用執行封裝[dotnet pack](/dotnet/core/tools/dotnet-pack)命令：</span><span class="sxs-lookup"><span data-stu-id="3f9eb-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="3f9eb-133">使用 NuGet 封裝上傳[dotnet nuget 發行](/dotnet/core/tools/dotnet-nuget-push)命令：</span><span class="sxs-lookup"><span data-stu-id="3f9eb-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="3f9eb-134">當使用`blazorlib`範本中，靜態資源會包含在 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="3f9eb-135">程式庫取用者會自動接收指令碼和樣式表，讓取用者不一定要以手動方式安裝的資源。</span><span class="sxs-lookup"><span data-stu-id="3f9eb-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
