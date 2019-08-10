---
title: ASP.NET Core Razor 元件類別庫
author: guardrex
description: 探索如何將元件包含在來自外部元件程式庫的 Blazor 應用程式中。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948438"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="f5184-103">ASP.NET Core Razor 元件類別庫</span><span class="sxs-lookup"><span data-stu-id="f5184-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="f5184-104">依[Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="f5184-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="f5184-105">元件可以在跨專案的[Razor 類別庫 (RCL)](xref:razor-pages/ui-class)中共用。</span><span class="sxs-lookup"><span data-stu-id="f5184-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="f5184-106">*Razor 元件類別庫*可以包含在:</span><span class="sxs-lookup"><span data-stu-id="f5184-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="f5184-107">方案中的另一個專案。</span><span class="sxs-lookup"><span data-stu-id="f5184-107">Another project in the solution.</span></span>
* <span data-ttu-id="f5184-108">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f5184-108">A NuGet package.</span></span>
* <span data-ttu-id="f5184-109">參考的 .NET 程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5184-109">A referenced .NET library.</span></span>

<span data-ttu-id="f5184-110">就像元件是一般的 .NET 類型, RCL 所提供的元件是一般的 .NET 元件。</span><span class="sxs-lookup"><span data-stu-id="f5184-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="f5184-111">建立 RCL</span><span class="sxs-lookup"><span data-stu-id="f5184-111">Create an RCL</span></span>

<span data-ttu-id="f5184-112">遵循<xref:blazor/get-started>文章中的指導方針, 設定您的環境以進行 Blazor。</span><span class="sxs-lookup"><span data-stu-id="f5184-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5184-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5184-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f5184-114">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="f5184-114">Create a new project.</span></span>
1. <span data-ttu-id="f5184-115">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f5184-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f5184-116">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f5184-116">Select **Next**.</span></span>
1. <span data-ttu-id="f5184-117">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="f5184-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="f5184-118">本主題中的範例會使用專案名稱`MyComponentLib1`。</span><span class="sxs-lookup"><span data-stu-id="f5184-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="f5184-119">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f5184-119">Select **Create**.</span></span>
1. <span data-ttu-id="f5184-120">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="f5184-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="f5184-121">選取 [ **Razor 類別庫**] 範本。</span><span class="sxs-lookup"><span data-stu-id="f5184-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="f5184-122">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f5184-122">Select **Create**.</span></span>
1. <span data-ttu-id="f5184-123">將 RCL 新增至方案:</span><span class="sxs-lookup"><span data-stu-id="f5184-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="f5184-124">以滑鼠右鍵按一下方案。</span><span class="sxs-lookup"><span data-stu-id="f5184-124">Right-click the solution.</span></span> <span data-ttu-id="f5184-125">選取 [**加入** > **現有專案**]。</span><span class="sxs-lookup"><span data-stu-id="f5184-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="f5184-126">流覽至 RCL 的專案檔。</span><span class="sxs-lookup"><span data-stu-id="f5184-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="f5184-127">選取 RCL 的專案檔 ( *.csproj*)。</span><span class="sxs-lookup"><span data-stu-id="f5184-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="f5184-128">從應用程式新增參考 RCL:</span><span class="sxs-lookup"><span data-stu-id="f5184-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="f5184-129">以滑鼠右鍵按一下應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="f5184-129">Right-click the app project.</span></span> <span data-ttu-id="f5184-130">選取 [**新增** > **參考**]。</span><span class="sxs-lookup"><span data-stu-id="f5184-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="f5184-131">選取 [RCL] 專案。</span><span class="sxs-lookup"><span data-stu-id="f5184-131">Select the RCL project.</span></span> <span data-ttu-id="f5184-132">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f5184-132">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f5184-133">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f5184-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="f5184-134">使用**Razor 類別庫**範本 (`razorclasslib`) 搭配命令 shell 中的[dotnet new](/dotnet/core/tools/dotnet-new)命令。</span><span class="sxs-lookup"><span data-stu-id="f5184-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="f5184-135">在下列範例中, 會建立名為`MyComponentLib1`的 RCL。</span><span class="sxs-lookup"><span data-stu-id="f5184-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="f5184-136">執行命令時, `MyComponentLib1`會自動建立保存的資料夾:</span><span class="sxs-lookup"><span data-stu-id="f5184-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="f5184-137">若要將程式庫新增至現有的專案, 請在命令 shell 中使用[dotnet add reference](/dotnet/core/tools/dotnet-add-reference)命令。</span><span class="sxs-lookup"><span data-stu-id="f5184-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="f5184-138">在下列範例中, 會將 RCL 新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="f5184-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="f5184-139">從應用程式的專案資料夾中, 使用程式庫的路徑執行下列命令:</span><span class="sxs-lookup"><span data-stu-id="f5184-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="f5184-140">用戶端應用程式不支援 RCLs</span><span class="sxs-lookup"><span data-stu-id="f5184-140">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="f5184-141">在目前的 ASP.NET Core 3.0 Preview 中, Razor 類別庫與 Blazor 用戶端應用程式不相容。</span><span class="sxs-lookup"><span data-stu-id="f5184-141">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="f5184-142">針對 Blazor 用戶端應用程式, 請在命令介面中使用範本所`blazorlib`建立的 Blazor 元件程式庫:</span><span class="sxs-lookup"><span data-stu-id="f5184-142">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="f5184-143">使用範本的`blazorlib`元件庫可以包含靜態檔案, 例如影像、JavaScript 和樣式表單。</span><span class="sxs-lookup"><span data-stu-id="f5184-143">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="f5184-144">在建立期間, 靜態檔案會內嵌到建立的元件檔 ( *.dll*) 中, 以允許取用元件, 而不必擔心如何包含其資源。</span><span class="sxs-lookup"><span data-stu-id="f5184-144">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="f5184-145">包含在`content`目錄中的任何檔案都會標示為內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="f5184-145">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="f5184-146">使用程式庫元件</span><span class="sxs-lookup"><span data-stu-id="f5184-146">Consume a library component</span></span>

<span data-ttu-id="f5184-147">若要使用另一個專案中的程式庫中所定義的元件, 請使用下列其中一種方法:</span><span class="sxs-lookup"><span data-stu-id="f5184-147">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="f5184-148">使用命名空間的完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="f5184-148">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="f5184-149">使用 Razor 的[ \@using](xref:mvc/views/razor#using)指示詞。</span><span class="sxs-lookup"><span data-stu-id="f5184-149">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="f5184-150">個別元件可以依名稱新增。</span><span class="sxs-lookup"><span data-stu-id="f5184-150">Individual components can be added by name.</span></span>

<span data-ttu-id="f5184-151">在下列範例中, `MyComponentLib1`是`SalesReport`包含元件的元件庫。</span><span class="sxs-lookup"><span data-stu-id="f5184-151">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="f5184-152">`SalesReport`元件可以使用其完整型別名稱和命名空間來加以參考:</span><span class="sxs-lookup"><span data-stu-id="f5184-152">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="f5184-153">如果使用`@using`指示詞將程式庫帶入範圍, 也可以參考此元件:</span><span class="sxs-lookup"><span data-stu-id="f5184-153">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="f5184-154">將指示詞包含在最上層的 *_Import razor*檔案中, 以將程式庫的元件提供給整個專案。 `@using MyComponentLib1`</span><span class="sxs-lookup"><span data-stu-id="f5184-154">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="f5184-155">將指示詞新增至任何層級的 *_Import razor*檔案, 以將命名空間套用至資料夾內的單一頁面或一組頁面。</span><span class="sxs-lookup"><span data-stu-id="f5184-155">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="f5184-156">組建、封裝和寄送至 NuGet</span><span class="sxs-lookup"><span data-stu-id="f5184-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="f5184-157">因為元件程式庫是標準的 .NET 程式庫, 所以封裝和傳送至 NuGet 的方式與將任何程式庫封裝和傳送至 NuGet 的方式並無不同。</span><span class="sxs-lookup"><span data-stu-id="f5184-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="f5184-158">封裝是使用命令 shell 中的[dotnet pack](/dotnet/core/tools/dotnet-pack)命令來執行:</span><span class="sxs-lookup"><span data-stu-id="f5184-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="f5184-159">使用命令 shell 中的[dotnet NuGet publish](/dotnet/core/tools/dotnet-nuget-push)命令, 將封裝上傳至 NuGet:</span><span class="sxs-lookup"><span data-stu-id="f5184-159">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="f5184-160">使用`blazorlib`範本時, 靜態資源會包含在 NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="f5184-160">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="f5184-161">程式庫取用者會自動接收腳本和樣式表單, 因此取用者不需要手動安裝資源。</span><span class="sxs-lookup"><span data-stu-id="f5184-161">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="f5184-162">建立具有靜態資產的 Razor 元件類別庫</span><span class="sxs-lookup"><span data-stu-id="f5184-162">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="f5184-163">RCL 可以包含靜態資產。</span><span class="sxs-lookup"><span data-stu-id="f5184-163">An RCL can include static assets.</span></span> <span data-ttu-id="f5184-164">使用該程式庫的任何應用程式都可以使用靜態資產。</span><span class="sxs-lookup"><span data-stu-id="f5184-164">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="f5184-165">如需詳細資訊，請參閱 <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>。</span><span class="sxs-lookup"><span data-stu-id="f5184-165">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5184-166">其他資源</span><span class="sxs-lookup"><span data-stu-id="f5184-166">Additional resources</span></span>

* <xref:razor-pages/ui-class>
