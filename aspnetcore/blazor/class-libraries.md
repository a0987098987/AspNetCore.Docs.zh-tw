---
title: ASP.NET核心剃刀元件類庫
author: guardrex
description: 瞭解如何從外部元件庫中將元件Blazor包含在應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/class-libraries
ms.openlocfilehash: f2cc57638922bd1f6ab036adb2ed37209d14c5b0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218762"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="9e53c-103">ASP.NET核心剃刀元件類庫</span><span class="sxs-lookup"><span data-stu-id="9e53c-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="9e53c-104">由[西蒙·蒂姆斯](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="9e53c-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="9e53c-105">元件可以在跨專案的[Razor 類庫 (RCL)](xref:razor-pages/ui-class)中共用。</span><span class="sxs-lookup"><span data-stu-id="9e53c-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="9e53c-106">*Razor 元件類庫*可以從:</span><span class="sxs-lookup"><span data-stu-id="9e53c-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="9e53c-107">解決方案中的另一個專案。</span><span class="sxs-lookup"><span data-stu-id="9e53c-107">Another project in the solution.</span></span>
* <span data-ttu-id="9e53c-108">NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="9e53c-108">A NuGet package.</span></span>
* <span data-ttu-id="9e53c-109">引用的 .NET 庫。</span><span class="sxs-lookup"><span data-stu-id="9e53c-109">A referenced .NET library.</span></span>

<span data-ttu-id="9e53c-110">正如元件是一般的 .NET 類型一樣,RCL 提供的元件是普通 .NET 程式集。</span><span class="sxs-lookup"><span data-stu-id="9e53c-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="9e53c-111">建立 RCL</span><span class="sxs-lookup"><span data-stu-id="9e53c-111">Create an RCL</span></span>

<span data-ttu-id="9e53c-112">按照本文中的<xref:blazor/get-started>指南為 Blazor 配置環境。</span><span class="sxs-lookup"><span data-stu-id="9e53c-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9e53c-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e53c-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="9e53c-114">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="9e53c-114">Create a new project.</span></span>
1. <span data-ttu-id="9e53c-115">選擇**Razor 函式庫**。</span><span class="sxs-lookup"><span data-stu-id="9e53c-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="9e53c-116">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9e53c-116">Select **Next**.</span></span>
1. <span data-ttu-id="9e53c-117">在 **"創建新 Razor 類庫**"對話框中,選擇 **"創建**"。</span><span class="sxs-lookup"><span data-stu-id="9e53c-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="9e53c-118">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="9e53c-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="9e53c-119">這個主題中的範例使用項目名稱`MyComponentLib1`。</span><span class="sxs-lookup"><span data-stu-id="9e53c-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="9e53c-120">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9e53c-120">Select **Create**.</span></span>
1. <span data-ttu-id="9e53c-121">新增 RCL 新增到解決方案:</span><span class="sxs-lookup"><span data-stu-id="9e53c-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="9e53c-122">右鍵單擊解決方案。</span><span class="sxs-lookup"><span data-stu-id="9e53c-122">Right-click the solution.</span></span> <span data-ttu-id="9e53c-123">選擇 **「添加** > **現有專案**」。</span><span class="sxs-lookup"><span data-stu-id="9e53c-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="9e53c-124">導航到 RCL 的專案檔。</span><span class="sxs-lookup"><span data-stu-id="9e53c-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="9e53c-125">選擇 RCL 的專案檔 *(.csproj*)。</span><span class="sxs-lookup"><span data-stu-id="9e53c-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="9e53c-126">從應用程式新增 RCL 的參考:</span><span class="sxs-lookup"><span data-stu-id="9e53c-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="9e53c-127">右鍵單擊應用專案。</span><span class="sxs-lookup"><span data-stu-id="9e53c-127">Right-click the app project.</span></span> <span data-ttu-id="9e53c-128">選擇 **「添加** > **參考**」。</span><span class="sxs-lookup"><span data-stu-id="9e53c-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="9e53c-129">選擇 RCL 專案。</span><span class="sxs-lookup"><span data-stu-id="9e53c-129">Select the RCL project.</span></span> <span data-ttu-id="9e53c-130">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="9e53c-130">Select **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="9e53c-131">如果在從樣本產生 RCL 時選擇了 **「支援頁面和檢視**」複選框,則還會將 *_Imports.razor*檔案添加到生成的專案的根目錄,其中包含以下內容,以啟用 Razor 元件創作:</span><span class="sxs-lookup"><span data-stu-id="9e53c-131">If the **Support pages and views** check box is selected when generating the RCL from the template, then also add an *_Imports.razor* file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> <span data-ttu-id="9e53c-132">手動將檔添加到生成的專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="9e53c-132">Manually add the file the root of the generated project.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="9e53c-133">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9e53c-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="9e53c-134">在命令 shell`razorclasslib`中使用**Razor 函式庫**樣本 ( ) 與[dotnet 新](/dotnet/core/tools/dotnet-new)命令一起。</span><span class="sxs-lookup"><span data-stu-id="9e53c-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="9e53c-135">在下面的示例中,將創建名為`MyComponentLib1`的 RCL。</span><span class="sxs-lookup"><span data-stu-id="9e53c-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="9e53c-136">執行命令時自動`MyComponentLib1`建立保留的資料夾:</span><span class="sxs-lookup"><span data-stu-id="9e53c-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

   > [!NOTE]
   > <span data-ttu-id="9e53c-137">如果從`-s|--support-pages-and-views`樣本產生 RCL 時使用交換機,則還會將 *_Imports.razor*檔案加入到產生的專案的根目錄,其中包含以下內容,以啟用 Razor 元件創作:</span><span class="sxs-lookup"><span data-stu-id="9e53c-137">If the `-s|--support-pages-and-views` switch is used when generating the RCL from the template, then also add an *_Imports.razor* file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > <span data-ttu-id="9e53c-138">手動將檔添加到生成的專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="9e53c-138">Manually add the file the root of the generated project.</span></span>

1. <span data-ttu-id="9e53c-139">要將函式庫加入到現有專案,請使用[dotnet 在](/dotnet/core/tools/dotnet-add-reference)命令 shell 加入引用命令。</span><span class="sxs-lookup"><span data-stu-id="9e53c-139">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="9e53c-140">在下面的示例中,RCL將添加到應用中。</span><span class="sxs-lookup"><span data-stu-id="9e53c-140">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="9e53c-141">使用函式庫的路徑從應用程式的項目資料夾中執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="9e53c-141">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="9e53c-142">使用函式庫元件</span><span class="sxs-lookup"><span data-stu-id="9e53c-142">Consume a library component</span></span>

<span data-ttu-id="9e53c-143">為了使用在另一個專案中的庫中定義的元件,請使用以下任一方法:</span><span class="sxs-lookup"><span data-stu-id="9e53c-143">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="9e53c-144">使用完整類型名稱與命名空間。</span><span class="sxs-lookup"><span data-stu-id="9e53c-144">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="9e53c-145">使用 Razor[\@的使用](xref:mvc/views/razor#using)指令。</span><span class="sxs-lookup"><span data-stu-id="9e53c-145">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="9e53c-146">可以按名稱添加單個元件。</span><span class="sxs-lookup"><span data-stu-id="9e53c-146">Individual components can be added by name.</span></span>

<span data-ttu-id="9e53c-147">在以下範例中,`MyComponentLib1`是包含元件的`SalesReport`元件庫。</span><span class="sxs-lookup"><span data-stu-id="9e53c-147">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="9e53c-148">可以使用`SalesReport`使用一個命名空間的全類型名稱引用此元件:</span><span class="sxs-lookup"><span data-stu-id="9e53c-148">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="9e53c-149">如果函式庫被`@using`引入具有指令的範圍,也可以參考該元件:</span><span class="sxs-lookup"><span data-stu-id="9e53c-149">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="9e53c-150">將`@using MyComponentLib1`指令包含在頂級 *_Import.razor*檔中,以使庫的元件可用於整個專案。</span><span class="sxs-lookup"><span data-stu-id="9e53c-150">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="9e53c-151">將指令添加到任何級別的 *_Import.razor*檔,以將命名空間應用於資料夾中的單個頁面或頁面集。</span><span class="sxs-lookup"><span data-stu-id="9e53c-151">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="9e53c-152">使用靜態資產建立 Razor 元件類庫</span><span class="sxs-lookup"><span data-stu-id="9e53c-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="9e53c-153">RCL 可以包括靜態資產。</span><span class="sxs-lookup"><span data-stu-id="9e53c-153">An RCL can include static assets.</span></span> <span data-ttu-id="9e53c-154">靜態資產可用於使用庫的任何應用。</span><span class="sxs-lookup"><span data-stu-id="9e53c-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="9e53c-155">如需詳細資訊，請參閱 <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>。</span><span class="sxs-lookup"><span data-stu-id="9e53c-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="9e53c-156">建置、打包及運送到 NuGet</span><span class="sxs-lookup"><span data-stu-id="9e53c-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="9e53c-157">因為元件庫是標準的 .NET 庫,因此打包並將其運送到 NuGet 與打包和將任何庫運送到 NuGet 沒有什麼不同。</span><span class="sxs-lookup"><span data-stu-id="9e53c-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="9e53c-158">使用命令 shell 中的[dotnet pack](/dotnet/core/tools/dotnet-pack)命令執行封包:</span><span class="sxs-lookup"><span data-stu-id="9e53c-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="9e53c-159">使用命令 shell 中的[dotnet nuget 推送](/dotnet/core/tools/dotnet-nuget-push)指令將包上載到 NuGet。</span><span class="sxs-lookup"><span data-stu-id="9e53c-159">Upload the package to NuGet using the [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push) command in a command shell.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e53c-160">其他資源</span><span class="sxs-lookup"><span data-stu-id="9e53c-160">Additional resources</span></span>

* <xref:razor-pages/ui-class>
* [<span data-ttu-id="9e53c-161">新增 XML 連結器設定檔到函式庫</span><span class="sxs-lookup"><span data-stu-id="9e53c-161">Add an XML linker configuration file to a library</span></span>](xref:host-and-deploy/blazor/configure-linker#add-an-xml-linker-configuration-file-to-a-library)
