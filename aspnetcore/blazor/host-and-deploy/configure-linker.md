---
title: 設定 ASP.NET Core 的連結器Blazor
author: guardrex
description: 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/configure-linker
ms.openlocfilehash: 76af450df70fe666ea1b951cb4b41696057c5e67
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243573"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="539e5-103">設定 ASP.NET Core 的連結器Blazor</span><span class="sxs-lookup"><span data-stu-id="539e5-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="539e5-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="539e5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

Blazor<span data-ttu-id="539e5-105">WebAssembly 會在組建期間執行[中繼語言（IL）](/dotnet/standard/managed-code#intermediate-language--execution)連結，以從應用程式的輸出元件修剪不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="539e5-105"> WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="539e5-106">在 Debug 設定中建立時，會停用連結器。</span><span class="sxs-lookup"><span data-stu-id="539e5-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="539e5-107">應用程式必須在發行設定中建立，才能啟用連結器。</span><span class="sxs-lookup"><span data-stu-id="539e5-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="539e5-108">我們建議您在部署 WebAssembly apps 時建立發行 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="539e5-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="539e5-109">連結應用程式會優化大小，但可能會有不利的影響。</span><span class="sxs-lookup"><span data-stu-id="539e5-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="539e5-110">使用反映或相關動態功能的應用程式可能會在修剪時中斷，因為連結器不知道此動態行為，而且無法判斷在執行時間的反映需要何種類型。</span><span class="sxs-lookup"><span data-stu-id="539e5-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="539e5-111">若要修剪這類應用程式，連結器必須通知程式碼中的反映所需的任何類型，以及應用程式所相依的封裝或架構。</span><span class="sxs-lookup"><span data-stu-id="539e5-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="539e5-112">若要確保已修剪的應用程式在部署後能正常運作，請務必在開發時經常測試應用程式的發行組建。</span><span class="sxs-lookup"><span data-stu-id="539e5-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="539e5-113">您 Blazor 可以使用下列 MSBuild 功能來設定應用程式的連結：</span><span class="sxs-lookup"><span data-stu-id="539e5-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="539e5-114">使用[MSBuild 屬性](#control-linking-with-an-msbuild-property)來設定全域連結。</span><span class="sxs-lookup"><span data-stu-id="539e5-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="539e5-115">使用[設定檔](#control-linking-with-a-configuration-file)來控制每個元件的連結。</span><span class="sxs-lookup"><span data-stu-id="539e5-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="539e5-116">使用 MSBuild 屬性的控制項連結</span><span class="sxs-lookup"><span data-stu-id="539e5-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="539e5-117">建立應用程式時，會啟用連結 `Release` 。</span><span class="sxs-lookup"><span data-stu-id="539e5-117">Linking is enabled when an app is built in `Release` configuration.</span></span> <span data-ttu-id="539e5-118">若要變更此項，請 `BlazorWebAssemblyEnableLinking` 在專案檔中設定 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="539e5-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="539e5-119">使用組態檔控制連結</span><span class="sxs-lookup"><span data-stu-id="539e5-119">Control linking with a configuration file</span></span>

<span data-ttu-id="539e5-120">透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：</span><span class="sxs-lookup"><span data-stu-id="539e5-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="LinkerConfig.xml" />
</ItemGroup>
```

<span data-ttu-id="539e5-121">`LinkerConfig.xml`:</span><span class="sxs-lookup"><span data-stu-id="539e5-121">`LinkerConfig.xml`:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="539e5-122">如需詳細資訊和範例，請參閱[資料格式（mono/連結器 GitHub 存放庫）](https://github.com/mono/linker/blob/master/docs/data-formats.md)。</span><span class="sxs-lookup"><span data-stu-id="539e5-122">For more information and examples, see [Data Formats (mono/linker GitHub repository)](https://github.com/mono/linker/blob/master/docs/data-formats.md).</span></span>

## <a name="add-an-xml-linker-configuration-file-to-a-library"></a><span data-ttu-id="539e5-123">將 XML 連結器設定檔加入至程式庫</span><span class="sxs-lookup"><span data-stu-id="539e5-123">Add an XML linker configuration file to a library</span></span>

<span data-ttu-id="539e5-124">若要設定特定程式庫的連結器，請將 XML 連結器設定檔新增至程式庫做為內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="539e5-124">To configure the linker for a specific library, add an XML linker configuration file into the library as an embedded resource.</span></span> <span data-ttu-id="539e5-125">內嵌資源的名稱必須與元件相同。</span><span class="sxs-lookup"><span data-stu-id="539e5-125">The embedded resource must have the same name as the assembly.</span></span>

<span data-ttu-id="539e5-126">在下列範例中，會將檔案 `LinkerConfig.xml` 指定為與程式庫的元件同名的內嵌資源：</span><span class="sxs-lookup"><span data-stu-id="539e5-126">In the following example, the `LinkerConfig.xml` file is specified as an embedded resource that has the same name as the library's assembly:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="LinkerConfig.xml">
    <LogicalName>$(MSBuildProjectName).xml</LogicalName>
  </EmbeddedResource>
</ItemGroup>
```

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="539e5-127">設定國際化的連結器</span><span class="sxs-lookup"><span data-stu-id="539e5-127">Configure the linker for internationalization</span></span>

<span data-ttu-id="539e5-128">根據預設， Blazor Blazor WebAssembly 應用程式的連結器設定會去除國際化資訊，但不包括明確要求的地區設定。</span><span class="sxs-lookup"><span data-stu-id="539e5-128">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="539e5-129">移除這些元件會將應用程式的大小降到最低。</span><span class="sxs-lookup"><span data-stu-id="539e5-129">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="539e5-130">若要控制要保留哪些國際化元件，請 `<BlazorWebAssemblyI18NAssemblies>` 在專案檔中設定 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="539e5-130">To control which I18N assemblies are retained, set the `<BlazorWebAssemblyI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyI18NAssemblies>{all|none|REGION1,REGION2,...}</BlazorWebAssemblyI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="539e5-131">區域值</span><span class="sxs-lookup"><span data-stu-id="539e5-131">Region Value</span></span>     | <span data-ttu-id="539e5-132">Mono 區域元件</span><span class="sxs-lookup"><span data-stu-id="539e5-132">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="539e5-133">包含的所有元件</span><span class="sxs-lookup"><span data-stu-id="539e5-133">All assemblies included</span></span> |
| `cjk`            | `I18N.CJK.dll`          |
| `mideast`        | `I18N.MidEast.dll`      |
| <span data-ttu-id="539e5-134">`none` (預設)</span><span class="sxs-lookup"><span data-stu-id="539e5-134">`none` (default)</span></span> | <span data-ttu-id="539e5-135">None</span><span class="sxs-lookup"><span data-stu-id="539e5-135">None</span></span>                    |
| `other`          | `I18N.Other.dll`        |
| `rare`           | `I18N.Rare.dll`         |
| `west`           | `I18N.West.dll`         |

<span data-ttu-id="539e5-136">使用逗號來分隔多個值（例如 `mideast,west` ）。</span><span class="sxs-lookup"><span data-stu-id="539e5-136">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="539e5-137">如需詳細資訊，請參閱[I18N： Pnetlib 國際化架構程式庫（mono/Mono GitHub 儲存機制）](https://github.com/mono/mono/tree/master/mcs/class/I18N)。</span><span class="sxs-lookup"><span data-stu-id="539e5-137">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="539e5-138">其他資源</span><span class="sxs-lookup"><span data-stu-id="539e5-138">Additional resources</span></span>

* <xref:blazor/webassembly-performance-best-practices#intermediate-language-il-linking>
